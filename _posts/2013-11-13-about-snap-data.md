---
title: "About SNAP Data"
layout: post
comments: true
excerpt:
categories: [mongodb, nosql, api]
---

## Overview

In my [last post](/2013/11/12/announcing-snapfinder/), I introduced SNAPfinder, a mobile-first web app, along with a separate framework and REST API, for helping low-income citizens find nearby participating SNAP stores (SNAP was formerly known as the "food stamps" program).

In this series, we will do a deep dive on the technology stack and tools involved, which, depending on the specific portion of the stack, included Node, MongoDB, Express and EJS templates, Twitter Bootstrap, and Digital Ocean.

## What is SNAP?

[SNAP](http://www.fns.usda.gov/snap/) stands for the Supplemental Nutrition Assistance Program mandated by the Federal Government and supervised by states to help millions of individuals and families who need financial assistance to buy food. Formerly known as the Food Stamp Program, SNAP provides an economic benefit as well as well as serving to eliminate hunger.

Today, instead of food stamps, eligible recipients are issued Electronic Benefit Transfer (EBT) cards. These cards can be used to purchase food and beverage items authorized by the USDA's SNAP program at retailers that accept EBT transactions.

## About SNAP data

The [USDA Food and Nutrition Service](http://www.fns.usda.gov/) maintains and publishes a list of retailers (vendors) across the country that welcome SNAP EBT customers. Data is stored in files in Comma Separated Value (CSV) format, which typically can be read by spreadsheet programs, such as Microsoft Excel. The file is updated regularly and can be downloaded from the following URL:

[http://www.snapretailerlocator.com/export/Nationwide.zip](http://www.snapretailerlocator.com/export/Nationwide.zip)

### Resource Format
The URL retrieves a zip file that stores a single CSV file with a .csv extension. The name of the file represents the publish date; for example:

`store_locations_2013_11_13.csv`

When we import the data into mongo, we will map the field names to more JSON friendly names as shown in the following table.

| CSV Field       | Map to |
|:----------------|:----------|
| Store_Name      | storeName |
| Longitude       | longitude |
| Latitude        | latitude  |
| Address         | address1  |
| Address Line #2 | address2  |
| City            | city      |
| State           | state     |
| Zip5            | zip5      |
| Zip4            | zip4      |

## Next steps

In the next post, we'll leverage two Cloud-host sevices for working with SNAP data. We'll use [Cloud9](https://c9.io/) to work with an editor and shell that we can use for downloading, importing, and querying mongo data, and we'll use [MongoLab](http://mongolab.com/) for storing the data that we harvest from the USDA.

We'll also discuss how we update the data as part of the import process to provide the fields necessary to support mongo geo queries. This will make it easy to answer queries, such as "find the stores within range of a particular location."
