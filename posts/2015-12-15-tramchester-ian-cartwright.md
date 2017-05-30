---
layout: post
title:  "Secure, Easy, and Visible: Why UK's Tramchester App Uses Snap"
description: "Tramchester UK used Snap CI to maintain and update their mobile web app which helps people navigate the Manchester tram network."
date:   2015-12-15
author: Suzie Prince
index-image-url: screenshots/tramchester/tramchester-mobile-app.png
index-image-alt: 'Tramchester UK Mobile App using Snap'
keywords: tramchester, ian cartwright, mobile development, snap ci, continuous delivery, ios development, android development
categories: mobile user-stories
---


[Tramchester](http://www.tramchester.co.uk/) is a mobile web application ThoughtWorks built to help people to navigate the the public tram network in Manchester, UK. Ian Cartwright, a Technical Principal Consultant on the project, talks to [The Pipeline](https://blog.snap-ci.com/) about the project and how Snap plays a part in delivering Tramchester to its users.

![tramchester mobile app](/assets/images/screenshots/tramchester/tramchester-mobile-app.png){: .screenshot .big}

### Can you tell us how Tramchester got started?

Tramchester started as a short 6 week innovation project which got out into the wild and we have been looking after it since then.

For the project we were working on, we were given six weeks to investigate a new tool or technology. The tool we investigated was Neo4j, a graph database. We used Neo4j to model the tram network and calculated the best route between two tram stations using a path finding algorithm.

We wanted working software, in a production environment, at the end of the six weeks. There's often a temptation to have manual steps or workarounds, but we were striving to show how something (like Neo4j) would work in the real world.

### What has happened since Tramchester went live?

We've made it much easier to release a new version. It is much less hassle now: we just get a new timetable and upload it. When we first did the project some things were hard coded.

### How often do you change Tramchester?

Maintenance activities are mostly triggered by tram timetable updates. The tram network continues to expand and they do a lot of work, so over time so we do have to accommodate those changes. When we see work happening on the trams, we will check for timetable changes.

### Was there anything special you wanted from your tools to support this?

We knew we wanted to manage our deployments as part of our build process. We really wanted to do one click deployments.

In Snap, we have a dev, a UAT and a production environment. We have a compile/pass stage, a test stage, an upload stage, deploy to dev, deploy to UAT and deploy to prod.

![tramchester snap pipeline](/assets/images/screenshots/tramchester/tramchester-snap-pipeline.png){: .screenshot .big}

### How has Snap CI helped you?

We started by using [Go](www.go.cd) hosted on AWS. As we no longer have a core team and we are only investing a few hours here and there, we wanted a solution that was cheaper than a AWS instance and easier to maintain. Moving to Snap gave us a service running in the cloud. It's ready to go, it's maintained, and it's kept up to date. From our point of view, it took away the need to maintain or patch our CI tool.

Also, we wanted a way to monitor build statuses in a secure way. With Snap, we got authentication for free and could easily and securely monitor the build status.

Security gets forgotten but it's really important. The [secure environmental variables](https://docs.snap-ci.com/pipeline/#environment-variables) Snap had for managing our credentials was really helpful. We were able to create a specific IAM user for Snap, and then lock it down so that it’s only got the bare minimum permissions it needs to deploy the app. Prior to this we had to do a lot of work to keep things secure. Security is now such a key focus: I consider this a "must have" feature. We are trying to maintain the principle of "least privilege" all the way through. Anything that makes it easier to manage credentials and security is very welcome to me.

### What are the other tools that helped you deliver Tramchester?

Cloud formation was important. We have it so we can recreate the entire environment from an empty VPC up to an application, working in about 15 minutes. As we tag our resources we can also use Amazon’s cost explorer to see the cost by environment and even build number. So we can easily say “How much does build number 100 cost vs build number 98?”

We are also using [CFNAssist](https://github.com/cartwrightian/cfnassist) which is an open source tool we wrote for another project. Originally when we did the deployments it was basically a bunch of shell scripts but now we have migrated to CFNAssist. It has let us delete a lot of the shell scripts and replace them with something that is a lot better tested.

### How was Tramchester received?

I think people were excited to see a transport app specifically for Manchester, of course there are now far more of those available now. The idea of something that helped plan a tram journey was appreciated as well, as the tram network gets more extensive there are more options and hence more need for something that can help out.

### What’s next from Tramchester?

As the tram network continues to expand we need to look at usability again, the list of stations is getting very long! We are also looking at how other data sets could be loaded into the path finding solution, perhaps other cities or other types of transport.
