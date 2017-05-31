---
layout: post
title:  "Simplify setting up Heroku deployments with OAuth"
description: Using Heroku OAuth makes Snap a single point of contact for deploying your code to an existing app on Heroku or even creating a new app for deployments.
date:   2013-09-13
author: Akshay Karle
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, heroku, continuous deployment, API, OAuth
categories: heroku oauth deployments
---

Snap now uses the new Platform API for Heroku. Specifically, we use [OAuth for Heroku](https://blog.heroku.com/archives/2013/7/22/oauth-for-platform-api-in-public-beta) for [setting up deployments]({{site.link.docs}}deployments/heroku_deployments). Using Heroku OAuth makes Snap a single point of contact for deploying your code to an existing app on Heroku or even creating a new app for deployments.

Using OAuth means that you no longer need to copy over the API key from Heroku. Authorize Snap on Heroku once and Snap takes care of the rest of the deployments.

 
Snap CI © 2017, ThoughtWorks
