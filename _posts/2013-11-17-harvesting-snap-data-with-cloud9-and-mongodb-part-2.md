---
title: "Harvesting SNAP Data with Cloud9 and MongoDB (Part 2)"
layout: post
comments: true
excerpt:
categories: [mongodb, nosql, api]
---

## Overview

In my [last post](/2013/11/15/harvesting-snap-data-with-cloud9-and-mongodb-part-1/), we set up accounts with MongoLab and Cloud9 to prepare to import SNAP data. In this post, we'll walk through the process of downloading and extracting the data, importing it into our mongo database, and performing a few simple queries.

## Recap

At this point, we have a newly created, empty database hosted in MongoLab. Our database user and connection information has been saved in our notes in Cloud9. Your actual connection will be different than the one shown in the following screenshots.

#### MongoLab database connection and user

![Database info](/assets/img/snap/mongolab-import-snap.png "Database info")

#### Cloud9 notes

![Cloud9 notes](/assets/img/snap/mongolab-create-db-7.png "Cloud9 notes")

## Accessing mongo

Let's confirm that we can access our database. In Cloud9, maximize the Terminal window and then start the mongo shell with the connection string for the database that we previously saved. Remember your connection string will be different.

```
mongo ds053198.mongolab.com:53198/snapdb -u apiuser -p snapdb
```

![Access Mongo](/assets/img/snap/mongolab-import-snap-02.png "Access Mongo")

If everything is correct, you will get a welcome message and prompt.

![Mongo Shell](/assets/img/snap/mongolab-import-snap-03.png "Mongo Shell")

Enter `ctrl-c` to exit the shell for now.

### Using aliases to make connecting easier

To make it easy to access the mongo shell with the same connection string each time, you can enter the following at the terminal prompt to create aliases for the mongo and mongoimport commands. The aliases will be appended to your Cloud9 workspace .bashrc file. The final command will reload .bashrc into the current environment so you don't have to log out and back in for the aliases to take effect.

    $ echo 'alias mongo="mongo ds053198.mongolab.com:53198/snapdb -u apiuser -p snapdb"' >> ~/.bashrc
    $ echo 'alias mongoimport="mongoimport -h ds053198.mongolab.com:53198 -d snapdb -u apiuser -p snapdb"' >> ~/.bashrc
    $ . ~/.bashrc

Note that the mongoimport doesn't work with the same url connection string format you can use for mongo, which has the database name appended after the last slash. With mongoimport the host portion used with -h is separate from the database name you specify with the -d option.

Confirm you can log into snapdb at MongoLab by entering `mongo` at the command line, as shown below:

![Aliases](/assets/img/snap/mongolab-import-snap-08.png "Aliases")

## Download SNAP Data

Now that we're set up with mongo, let's download the current SNAP data file using curl:

    $ curl -O www.snapretailerlocator.com/export/Nationwide.zip

![Download SNAP Data](/assets/img/snap/mongolab-import-snap-04.png "Download SNAP Data")

Unzip the downloaded file to extract the compressed CSV file:

    $ unzip -o Nationwide.zip

![Unzip SNAP Data](/assets/img/snap/mongolab-import-snap-05.png "Unzip SNAP Data")

Don't try to open the extracted CSV file into the Cloud9 editor -- it's too big! You can examine the top of the file to get a feel for what's in it using the head command:

    $ head store_locations_2013_10_29.csv

By default, you'll get a dump of the first ten lines of the file. You can see the first line is a header with field names for the data.

![Inspect CSV File](/assets/img/snap/mongolab-import-snap-06.png "Inspect CSV file")

## Import SNAP Data

Now we can import the data into our mongo database using the [mongoimport](https://docs.mongodb.org/manual/reference/program/mongoimport/#bin.mongoimport) utility, which can import data in either [json](https://json.org/), [csv](https://en.wikipedia.org/wiki/Comma-separated_values), [tsv](https://en.wikipedia.org/wiki/Tab-separated_values) formats.

The header field names are not in an object-friendly format, so we'll provide mongoimport with the actual field names we'd like to use when it stores the data. We'll store the field names in a variable that we'll use with the mongoimport command so that the command itself doesn't look so long.

    $ FIELDS="storeName,longitude,latitude,address1,address2,city,state,zip5,zip4,county"

Now we'll start the import process. This will take a couple of minutes.

    $ mongoimport --type csv --fields $FIELDS --collection stores --file store_locations_2013_10_29.csv

Note this requires setting up the mongoimport that we discussed earlier. If you didn't do that, you will also need to supply -h, -d, -u, and -p arguments for host, database, username, and password respectively.

If all is going well, you will see output as shown below:

![mongoimport](/assets/img/snap/mongolab-import-snap-09.png "mongoimport")

When mongoimport is finished, it will report the number of objects it imported, as shown below. For the most recent data, that's 249,961 stores.

![mongoimport results](/assets/img/snap/mongolab-import-snap-10.png "mongoimport results")

## Querying mongo

Now that we've imported data, we can go back into the mongo shell and take a look at it. Again, assuming you set up the mongo alias discussed earlier, simply enter mongo at the terminal prompt.

Enter `show collections` in the shell; you should see the newly created stores collection in the output. Enter `db.stores.stats()` to see the metadata associated with the stores collection. Your results should be similar to this:

![collection stats](/assets/img/snap/mongolab-import-snap-11.png "collection stats")

#### Listing stores

Enter `db.stores.find().limit(5).pretty()` to list the first 5 stores with nice formatting.

![list stores](/assets/img/snap/mongolab-import-snap-12.png "list stores")

#### Removing the header

You probably notice that the first document in the collection isn't a valid store -- it's the first line of csv file that we imported, which consisted of a header of field names.

There is an option for mongoimport that allows us to specify that the first line is a header, but unfortunately mongoimport will then use those field names instead of the ones we supplied.

We also could have processed the csv file to remove the first line before importing, but because it's a big file, I chose not to perform any type of operation that might have involved creating a temporary file or copy. For example, the following would have worked, but would have resulted in a copy of the file:

    tail =n +2 store_locations_2013-10-29.csv >> output.csv

An easy way to remove this document is remove it after it has been imported. We can remove it with a simple query like this:

    > db.stores.remove({ storeName: 'Store_Name' })

And confirm that it's gone:

![delete header](/assets/img/snap/mongolab-import-snap-13.png "delete header")

## Finding stores

Let's find stores in Chicago:

    > db.stores.find({ city: 'Chicago' }).pretty()

This will result in a list with a lot of results. So many, in fact, that they will not be returned all at once. The result of find is a cursor that can be iterated, and the shell will prompt you to type "it" if you wish to iterate to see more results, as shown here:

![iterate results](/assets/img/snap/mongolab-import-snap-14.png "iterate results")

Just how many results are there? You can find out in the shell either before you submit the find query:

    > db.stores.count({ city: 'Chicago' })

or by getting the count for the cursor that find returned:

    > db.stores.find({ city: 'Chicago' }).count()

Either method returns the same answer, which for the data I used was 2,402 stores.

If I had wanted to save the cursor in a variable, then I could have done something like this (which will still output results):

    > chicago_stores = db.stores.find({ city: 'Chicago' })
    ...
    > chicago_stores.count()
    2402


## Next steps

We now have data and we can perform queries, such as find the stores within a city or zip code, and with a bit of effort and math (see using the [Havesine formula](https://www.movable-type.co.uk/scripts/latlong.html)), we can compute distances to stores to find those within a desired range of a specific location.

However, mongo has great support for fast geo queries that we can exploit as long as we provide location data in a specific format. In our next post, we cover the post-import updates we perform to take advantage of this facility.
