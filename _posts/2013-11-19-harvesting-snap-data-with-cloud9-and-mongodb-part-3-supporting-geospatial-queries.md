---
title: "Harvesting SNAP Data with Cloud9 and MongoDB (Part 3) - Supporting Geospatial Queries"
layout: post
comments: true
excerpt:
categories: [mongodb, nosql, api]
---

## Overview

In my [last post](/2013/11/17/harvesting-snap-data-with-cloud9-and-mongodb-part-2/), we imported data into a mongo database stored in MongoLab's cloud. We used Cloud9 for the convenience of a ready-to-run environment for using the mongoimport utility and the mongo shell for testing a few queries.

In this post, we'll discuss performing an post-import update on our stores collection that allows us to exploit mongo's support for geospatial queries. This will support the use case in which we can find the stores that are closest to a particular location. For example, given a user's current location, we want to find participating SNAP retailers within a desired range, such as three miles.

Because the update logic is a bit unwieldy to type interactively in the mongo shell, we'll see how to use Cloud9 to save our update logic as a script that we can load and execute into the shell when we want to run it.

We'll use the same technique to create a query that we can load into the shell as a function that we can execute with a few arguments so that we can easily run parameterized queries based on a location and desired range to find nearby stores.

## Geospatial queries with mongo

Mongo provides a number of ways of supporting various types of geospatial queries. You can learn more about it starting [here](http://docs.mongodb.org/manual/applications/geospatial-indexes/). For our use case, we can use mongo's [2d indexes](http://docs.mongodb.org/manual/core/2d/) to find distances between points based on geometric projections to a flat Euclidean plane, which is suitable for relatively small distances. If our requirements were more stringent and involved greater distances, it would be better to use mongo's 2dsphere support.

The data set that we imported from the USDA included longitude and latitude fields. We need to convert that data into a location field of a type expected by mongo to support indexing on geo data.

For each store document stored in our stores collection, we'll add a new field that uses the store's latitude and longitude formatted as coordinate pairs as follows:

~~~
{ loc: [ lng, lat ] }
~~~

With this specific coordinate format, it isn't necessary but we will want to create a 2d index on these points to improve query performance, as discussed in the next section.

## Geo support update script

Right-click on the snapdata folder in the Cloud9 Workspace Files pane and create a new file. Call it `postimport.js`. Double-click to open it in its own tab. Enter the following script:

~~~
print('updating snapdb for geo query support...');
db.stores.find().forEach(function(store) {
  db.stores.update(
    { _id: store._id },
    { $set: { loc: [store.longitude, store.latitude] }
  });
});

print('indexing...');
db.stores.ensureIndex({ "loc": "2d" });
~~~

You can ignore the little warning icons in Cloud9 that tell you you're using an undeclared variable. We'll load the script into the mongo shell where db is a valid object. You should see something similar to the following:

![postimport](/assets/img/snap/mongo-postimport-01.png "postimport")

What the script does is apply an update function for every store document in the stores collection. After mongo is finished iterating through the entire collection, we then use the ensureIndex function to apply an index on GeoJSON points that we added.

#### Running the update script

In the Cloud9 Terminal, start the mongo shell; then load our newly created update script:

    > load('postimport.js')

This will load and execute the script, as shown in the screenshot below. Be warned that this process will take around five minutes! There are different strategies we can pursue to make this a faster process. One would involve running our script on the server instead of from the shell in Cloud9 -- but that would require administrative permission not available with the shared plan we're using with MongoLab; other possibilities include transforming the data before it was imported, but for this blog series I wanted to keep the focus on things we can do to manipulate data within mongo.

![load script](/assets/img/snap/mongo-postimport-02.png "load script")

#### Verify the update

In the mongo shell, run the following query to examine a store document:

    > db.stores.find().pretty()

From the output, we can verify that our collection has indeed been updated

## Creating a geo query

Now that the update script has run and added a new field to each store, our stores collection supports geospatial queries.

We can create another script to make it easy to query the stores collection with a function that accepts coordinates and a desired range. Create a new file called queries.js and add the following code:

~~~
// find stores within a target range (in miles)
function findStoresInRange(lat, lng, range, limit) {
    return db.runCommand({
        geoNear: 'stores',
        near: [lng, lat],          // longitude first
        spherical: true,
        distanceMultiplier: 3959,  // return results in miles
        maxDistance: range / 3950, // range in radians
        limit: limit || 100        // 100 is the default anyway
    });
}
~~~

This function allows us to easily make parameterized queries. From the mongo shell, we can load this function using `load('queries.js')`.

At this point, you should see something like the following:

![load queries](/assets/img/snap/mongo-postimport-03.png "load queries")

We need to supply a latitude and longitude as a reference point for the query. We could use the stores collection itself to find a store, get its location, then run our query to find all the other SNAP stores close to it.

But we can easily use [Google's geocoding service](https://developers.google.com/maps/documentation/geocoding/) to give us the location coordinates for an address we submit.

Open another browser window and enter a query similar to this example, which uses the address of Hacker Dojo in Mountain View. Use `+` in place of spaces to separate the address components.

<http://maps.googleapis.com/maps/api/geocode/json?sensor=false&address=599+fairchild+dr,mountain+view,ca>

Look for the geometry property in the results and note the latitude and longitude values.

![geocode result](/assets/img/snap/mongo-postimport-04.png "geocode result")

#### Running our query

With our queries script loaded into the shell (described in the last section), we can now execute a query like the one shown below:

    > q = findStoresInRange(37.4028344, -122.0496017, 3, 100)

![query](/assets/img/snap/mongo-postimport-05.png "query")

Stores will be printed in the shell, but since we saved the result in a variable, we can take examine it in pieces. To display how many results were returned, enter:

    > q.results.length

The results are sorted in distance order in units of miles (since our function provided a `distanceMultiplier`, which multiplies radians by the radius of the earth in miles to give us unit-friendly results).

We can take a look at individual results, such as the first one, like this:

    > q.results[0]

or the last:

    > q.results[64]

![examining results](/assets/img/snap/mongo-postimport-06.png "examining results")


## Next steps

In the past few posts, we covered the process of importing csv data into a mongo database. To save the trouble of setting up our own environment, we leveraged two excellent cloud services: [MongoLab](http://mongo.com/) for hosting the database, and [Cloud9](https://c9.io/) for providing a development environment that allowed us to interactively work with SNAP data.

In this post, we discussed how we tuned the data to support queries that allow us to locate stores within a desired range of a specific location, and we took a look at what those queries look like.

We now have a solid foundation upon which to build an API to help citizens find SNAP stores near to them.

In the next few posts, we'll discuss:

* Building an API layer to responsd to various REST requests for data using [Node](http://nodejs.org/)
* Building a mobile-first web app to provide a user interface that consumes the API using Node, [Express](http://expressjs.com/){:target="_blank"}, [EJS templates for Node](https://github.com/visionmedia/ejs), and [Bootstrap](http://getbootstrap.com/)
* Moving the API and app into production and setting up an automated job to harvest updated SNAP data on a daily basis
* The open source projects on GitHub that provide a working SNAP API and app; a bit about the architecture and design philosophy; and details how to fork them and keep your fork current, contribute changes, and submit and follow issues.
