---
title: "The Digital Government Strategy in the Age of GitHub"
layout: post
comments: true
excerpt: "The Digital Government Strategy embraces elements of successful Silicon Valley startups and the GitHub revolution to improve access to data."
---

![Digital Government](/assets/img/digitalgov2-150x150.png "Digital Government")

If you haven’t yet heard about the [Digital Government Strategy](http://www.whitehouse.gov/sites/default/files/omb/egov/digital-government/digital-government-strategy.pdf), you might be a bit surprised by what you learn, and you will certainly be impressed. Of course, the federal government’s use of information technology and data has a long history, as does its investment in research into technological advances that ultimately become a part of the fabric of our daily lives (thank you, [DARPA](http://www.darpa.mil/WorkArea/DownloadAsset.aspx?id=2554)).

The reason why you will be impressed with the Digital Government Strategy, however, is the extensive and comprehensive commitment the federal government has made to embrace the elements of successful Silicon Valley startup culture and open, social, collaborative development to deliver information and services to the citizens of this country — anywhere, anytime, on any platform or device.

Our team paid a visit to [Lisa Schlosser](http://dorobekinsider.com/2011/06/01/dorobekinsider-kundra-names-schlosser-as-deputy-federal-cio/), Deputy Federal CIO, back in January, when I learned about how the federal mobility roadmap evolved into the broader digital government strategy. As I learned about her office’s efforts to promote improving citizen access to government data, I naively suggested that her office should consider promoting the use of GitHub as a means of fostering an open source development community to create the APIs and apps that could get data out to the public. Lisa graciously smiled at me and said I really needed to meet [Haley Van Dyck](http://fcw.com/articles/2012/09/30/rising-star-van-dyck-haley.aspx), a policy analyst on her team.

A few weeks later, I spent an hour on the phone with Haley. Frankly, I was blown away by how clear the federal vision is and how articulately Haley was advocating and championing ideas that sound normal for startups, open source developers, and members of the GitHub revolution — yet not at all what you expect to hear coming from the government. And, most impressive of all, this wasn’t just talk — the Digital Government Strategy was published not quite one year ago, and there are already success stories.

For example, take a look at [RFP-EZ](https://rfpez.sba.gov/). Aside from the clean, [Twitter Bootstrap](http://getbootstrap.com/)-styled interface, the most amazing part is the “Fork this site on Github!” link at the bottom of the page. Think and digest on that for just a moment.

Then take a look at the beta site for [MyUSA](https://my.usa.gov/) and note the Developers link at the bottom of the page. Yet another aspect of the Digital Government Strategy is to ensure that agency sites provide a developer link.

Well, you can’t spend an hour on the phone with someone like Haley and not be inspired by her contagious enthusiasm to be a part of history and make a contribution to your country. She pointed me out to [Data.gov](http://www.data.gov/), where I spent some time looking at raw datasets that have been published by various agencies. At her suggestion and with her encouragement, I focused on consuming a federal data source, providing a REST API that could be easily consumed by a mobile device, and open sourcing the project on GitHub.

At Data.gov, I found a link to federal challenges data and the current web portal at [Challenge.gov](http://challenge.gov/). This is actually a fascinating site to browse because there are actual cash rewards for submitting a winning entry to challenges posted by various agencies through Challenge.gov. I liked the idea, so I decided as a proof of concept to harvest the [existing RSS feed](http://challenge.gov/api/challenges.xml), and make these challenges available in an iPhone app.

The result of two weekends and a few evenings of effort resulted in an actual complete app and is shown in the following screenshot.

![Challgenge.gov app screenshot](/assets/img/challengegov-screenshot-sm.png "Challenge.gov app screenshot")

Challenge.gov iPhone app

The app is an [open source project on GitHub](https://github.com/itsource/challenge-ios). It provides similar functionality to the existing website. The app uses a REST API written in Node.js, hosted by Modulus.io in the Amazon cloud.

The [API is a separate GitHub project](https://github.com/itsource/challenge-api), which harvests the RSS feed hourly and stores the data in a MongoDB database hosted by MongoLab, also in the Amazon cloud. The hourly harvest job is kicked off using a cloud-based service at [CRON.io](http://cron.io/) (a project I recently joined). The rest of the API serves to respond to queries for data (for example, the [JSON equivalent](http://challengeapi-7312.onmodulus.net/challenges) of the source RSS feed).

Because there is already an existing Challenges.gov site and this weekend project was more about providing a proof of concept that just happened to turn out to be a complete working app, the project and license will be transferred to the GSA, the agency responsible for the site. However, it is really interesting to note that the government is actually encouraging people in the private sector to feel free to commercialize any of their products that consume open data.

Haley pointed me toward a major success story, [iTriage](https://www.itriagehealth.com/). In fact, the company did so well with its mobile app and site, they have since been acquired by Aetna. The mobile healthtech market is [projected to quadruple to $400 million by 2016](http://techcrunch.com/2011/12/16/aetna-itriage-healthagen/), so it makes sense that the government’s strategy is to not only increase efficiency within federal agencies and improve citizen access to information, but to stimulate successful commerce as well based on digital innovation.

I would encourage civic-minded developers and entrepreneurs to take a look at the Digital Government Strategy, become a part of a new and growing developer community, and become aware of opportunities to contribute to your country — and pursue financial rewards stemming from this historic opportunity. And if you’re looking for strategic guidance or a partner to work with to pursue cloud and mobile development, feel free to get in touch.