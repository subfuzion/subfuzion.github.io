---
title: "Announcing SNAPfinder"
layout: post
comments: true
excerpt: "'Big data is (bovine excrement)' — or so says Harper Reed, former CTO for the Obama for America campaign..."
---

![SNAP Logo](/assets/img/snaplogo.png " SNAP Logo")

Back in May I [blogged](/2013/05/29/kicking-off-an-open-source-project-with-the-gsa/) about brainstorming with Gwynne Kostin, Director of Digital Services Innovation Center at the GSA about kicking off an open source project to support the [Digital Government Strategy](http://www.whitehouse.gov/sites/default/files/omb/egov/digital-government/digital-government-strategy.pdf).

Gwynne introduced me to Gray Brooks, Sr. API Strategist at the GSA Digital Services Innovation Center, and together the three of us got together on a number of weekly Google Hangouts to bounce our ideas off each other until we had a strategy in place for what we wanted to accomplish around Gwynne’s idea to help out low income citizens. These were our goals:

* To make a positive contribution to society by leveraging the Government’s commitment to Open Data
* To demonstrate a successful model of Open Source collaboration between a Federal Agency and the private sector development community
* To provide a well-documented example of leveraging a technology stack based on [Node](http://nodejs.org/) and [MongoDB](http://www.mongodb.org/) as a particularly effective way of building high-performance, scalable, cloud-hosted REST APIs for broad consumption.

Currently, the [USDA Food and Nutrition Service](http://www.fns.usda.gov/) administers [SNAP](http://www.fns.usda.gov/snap/). If a person wants to find retail stores participating in SNAP, the USDA hosts a flash-based webpage at [www.snapretailerlocator.com](http://www.snapretailerlocator.com/). We wanted to leverage the same USDA data, and architecturally, we wanted to provide a reference implementation that demonstrated a clear separation of responsibilities between:

* an open source framework for dealing with SNAP data
* a REST API that exposes SNAP functionality to be consumed by app developers
* a [mobile-first](http://zurb.com/word/mobile-first) web app that uses the API to provide a snappy (pun intended) UI

Today I’m pleased to announce [SNAPfinder](http://snapfinder.org/), available on the web and on your mobile phone at the following link:

[http://snapfinder.org/](http://snapfinder.org/)

In terms of actual development time, it took just under two weeks for two contributors (myself and [Tenny Susanto](http://tennysusantobi.blogspot.com/), Sr. Software Engineer at [Coupons.com](http://www.coupons.com/)) to get this out the door with three separate open source projects hosted on GitHub:

* [snapfinder](https://github.com/tonypujals/snapfinder) - a mobile-first web app that uses the API (Node, Express, EJS, Bootstrap 3)
* [snapfinder-api](https://github.com/tonypujals/snapfinder-api) - REST API for querying SNAP data (Node, Express)
* [snapfinder-lib](https://github.com/tonypujals/snapfinder-lib) - provides underlying framework for importing and querying SNAP data (Node, MongoDB)

The project includes an import job that harvests USDA data daily and imports retail stores in the Mongo database. Mongo provides special support for geo queries, which along with its support for JavaScript makes it very well-suited for pairing with Node for an extremely responsive and scalable API. The web app itself leverages also leverages Node and Express along with EJS templates, as well as Bootstrap 3 for the mobile-first UI support.

Over the next week, I plan to take a deeper dive into the technology and how we built the solution. In the meantime, I welcome you to check out the site and the various GitHub projects. I’m certain the USDA would welcome more contributors as we flesh out the analytics, enhance the UI and add a few more features. And, of course, all involved are certainly delighted to see Open Data project participation.
