---
layout: post
title:  "Setting up a CI server is sooo 2011"
description: Isn't it time your CI tool learnt about those conventions and decided to do the provisioning of the machine?
date:   2012-10-25
author: Badrinath Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github
categories:
---

You probably woke up this morning with a new idea for a web app. You want to build it and get it out in front of people as fast as possible to see if it gains any traction. You decided to pick a simple convention based web-framework, because this was not the time for you play around with all the options; it was time for you to build this thing and get it out the door and iterate. You decided to deploy to Heroku, because, man- they make it so easy to do.

Your team writes tests and run them locally - but you really can't be bothered to set up a CI server. Not even the 10 minutes it takes you to download one. Not the hour or so it will take you to get all the required libraries and dependencies installed. Not the 20 minutes it would take you to set up a build after all this. And you certainly do not want to baby sit the machine, managing upgrades and downtimes. That is the couple of hours a week that you just do not have - or even if you do, you would much rather spend learning a new  language or framework - or maybe even get some sleep?!

If your web application development framework follows conventions and if deploying your application to production is a single Git command you run from your terminal, isn't it time your CI tool learnt about those conventions and decided to do the provisioning of the machine, configuring libraries and setting up your CI pipeline- all out of the box without you having to tell it the same things over and over again? Isn't it 2012 already?

 
Snap CI © 2017, ThoughtWorks
