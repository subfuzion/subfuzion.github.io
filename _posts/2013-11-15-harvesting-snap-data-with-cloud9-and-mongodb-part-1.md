---
title: "Harvesting SNAP Data with Cloud9 and MongoDB (Part 1)"
layout: post
comments: true
excerpt:
categories: [mongodb, nosql, api]
---

## Overview

In my [last post](/2013/11/12/announcing-snapfinder/), I introduced SNAPfinder, a mobile-first web app, along with a separate framework and REST API, for helping low-income citizens find nearby participating SNAP stores (SNAP was formerly known as the "food stamps" program).

In this series, we will do a deep dive on the technology stack and tools involved, which, depending on the specific portion of the stack, included Node, MongoDB, Express and EJS templates, Twitter Bootstrap, and Digital Ocean.

In this post, we will cover the process of harvesting USDA SNAP CSV data. We will store data in MongoDB and augment it to support fast geo queries. These geo queries will be used to locate nearby SNAP stores from a particular location.

We will exploit two Cloud-hosted services to reduce the amount of effort it takes to create an environment for working with the data. For our purposes, the free plan of both services will be adequate.

* [Cloud9](http://c9.io/) is a Cloud-hosted IDE. We will use the shell environment it provides to perform the tasks necessary to download SNAP data and import it into a mongo database.

* [MongoLab](http://mongolab.com) provides a Cloud-hosted MongoDB service. We will use it to import and serve SNAP data to support our queries.

## Cloud9

Go to [Cloud9](https://c9.io/) and sign up using your [GitHub](https://github.com/) account.

#### Create a new workspace

![Create a Cloud9 workspace](/assets/img/snap/c9-create-new-workspace.png "Create a Cloud9 workspace")

#### Name your new workspace

Call it "snapdb" and select the "Custom" option (which gives you a barebones workspace with just a README.md file). Cloud9 will take a moment while it provisions your new cloud-hosted development workspace.

![Name your Cloud9 workspace](/assets/img/snap/c9-create-new-workspace-2.png "Name your Cloud9 workspace")

#### Open your new workspace

Click the "Start Editing" button. Your workspace will open in a new window (or tab) and initialize.

![Open workspace](/assets/img/snap/c9-create-new-workspace-3.png "Open workspace")

#### Prepare your workspace

Right-click on README.md and choose "Delete" from the context menu to get rid of it.

![Delete README](/assets/img/snap/c9-create-new-workspace-4.png "Delete README")

Right-click on the snapdata workspace folder and create a new file. Call it "notes.md". Double-click to open the file. We'll use the file to save notes about our mongo database that we'll create in the next step.

## MongoLab

#### Create a new database

Go to [MongoLab](http://mongolab.com) and sign up for an account. Once you're signed in, create a new database.

![Create database](/assets/img/snap/mongolab-create-db.png "Create database")

#### Create a new database - configuration options

Choose a free sandbox database on Amazon; name it "snapdb".

![Configure database](/assets/img/snap/mongolab-create-db-2.png "Configure database")

#### Open the database page

![Open database page](/assets/img/snap/mongolab-create-db-3.png "Open database page")

#### Add a database user

To access the database from a client, we need to create a database user. The database user credentials will be used when connecting to snapdb. Click the link that says "Click here".

![Create a database user](/assets/img/snap/mongolab-create-db-4.png "Create a database user")

#### Set database user credentials

Choose "apiuser" for the username and "snapdb" for the password.

![Set database user credentials](/assets/img/snap/mongolab-create-db-5.png "Set database user credentials")

#### Save the database information

You're done with database creation. Save the connection information and database credentials in your Cloud9 notes.

![Database info](/assets/img/snap/mongolab-create-db-6.png "Database info")

![Database notes](/assets/img/snap/mongolab-create-db-7.png "Database notes")

## Next steps

We now have everything in place to get SNAP data and loaded into a mongo database. In our next post, we'll walk through the steps of downloading, importing, transforming, and querying SNAP data interactively from the Cloud9 shell.

