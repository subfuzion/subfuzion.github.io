---
title: "A Gentle Introduction to Node - Part 1"
layout: post
comments: true
excerpt:
categories: [node, javascript, cloud9, api]
---

## Overview

There are plently of Node tutorials out there, so why write another one? The goal of this mini-primer is to provide enough of an introduction to Node and the relevant amount of JavaScript it takes to follow my blog posts. The primary audience for this post consists of those who have a reasonable degree of proficiency with at least one other programming language and web development (or a strong urge to achieve it) and an interest in gaining some exposure to Node.

## Why Node?

While Node might not be ideal for certain types of applications, I'm no apologist; if you're primarily concerned with building scalable REST APIs, it might be more useful to consider why not Node. The Node technology stack is seeing serious adoption by companies where performance and scalability are vital to their web platform, particularly as they expand their reach to a growing mobile audience. A who's who of successful Internet and enterprise companies adopting Node includes 

I'm a Node enthusiast for a number of reasons, which I will highlight throught this series. I first became excited about Node toward the end of 2009, but it wasn't until 2012 that I was able to fully embrace it as part of my primary development stack building REST APIs for a few mobile apps.

JavaScript and JSON are so strongly associated with the web development

So many others have written eloquently about Node, so I will try to be succinct about the essence of the platform. Node was designed from the beginning for scalability around the concept of non-blocking I/O, and the design was baked into every layer of Node and its supporting framework to ensure requests do not block. 

What does that mean? The vast majority of HTTP requests for a resource spend a significant portion, if not the majority, of their lifetime blocked while waiting to retrieve some kind of data, such as data from the file system, a database, over a socket, other web services, etc.

Most platforms handle each request in its own thread; therefore, a blocked request generally results in significant wasted system resources, adversely draining system performance and limiting the number of requests that can be served.

Node is designed not to waste these system resources because it doesn't create a new thread in response to each request; the code you write to service the request runs in the same thread for all of your requests. This seems very counter-intuitive, but the way that Node scales to hundreds of thousands of requests is by exploiting the premise that the majority of server code is not CPU-bound, but I/O-bound.

### The Event Loop

The code that you write is given an opportunity to run in what is called the Node event loop. It will run until it performs a request that results in waiting for I/O (files, databases, sockets, other web requests, etc), which ends its turn. The request for data includes supplying a callback that will do something with that data; when the data is available, the callback will be queued so that it can be run during a subsequent pass around the event loop.

This mechanism means that the precious system resources do not need to be allocated on a thread per request basis and expensive context switches are avoided. It also means that it's possible to write CPU-intensive code that starves the event loop and prevents other requests from being serviced. Node does not prevent pathological behavior on your part, but it does provide facilities to allow you to periodically "yield" cooperatively to the event loop to allow other callbacks a chance to execute.

There are many reasons why developers choose various platforms; choosing Node is generally a conscious decision to leverage a platform where scalability is the primary goal, whether or not JavaScript is the preferred programming language. For building web APIs around data and web services, where I/O is responsible for the greatest proportion of request latency, Node provides a compelling solution, particularly in light of its excellent package management system and growing number of useful packages available for Node development.
