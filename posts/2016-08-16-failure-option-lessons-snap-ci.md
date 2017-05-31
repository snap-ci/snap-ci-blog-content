---
layout: post
title:  "Failure is an Option - Lessons from Snap CI: Part 3 of 3"
description: "Lessons I learned the hard way about disappearing packages, rebuilding regularly, and more."
date:   2016-08-16
author: Jeff Norris
index-image-url: screenshots/failure-option/failure-option-3.png
index-image-alt: 'failure-lessons'
keywords: deploy software, continuous deployment, software deployment, continuous delivery, devops
categories: SOFTWARE-DELIVERY AGILE CONTINUOUS-DELIVERY
---
In [part two](https://blog.snap-ci.com/blog/2016/08/09/failure-option-feature-toggles-part-two/) of this series, I talked about lessons I learned from real-life experiences working on Mingle, an agile project management application. In this post, the third and final in the series, I'll talk about what I've learned working on my current product, Snap CI.

Snap is ThoughtWorks’ continuous integration and deployment-in-the-cloud service. There are two substantially different parts of Snap CI. There's the front end that users interact with, and the back end that builds, tests, and deploys our user’s software. The back end is very different than most software projects I have worked on because the product is a platform that can execute arbitrary code in a secure container. On most applications, if the user is able to execute arbitrary code on your server, it means you have been hacked; on ours, it means we are working properly.

Creating a build platform for Snap involves creating build hosts that run all our builds as well as build containers that run individual builds for our customers. The structure and content of both have to change on a regular basis to provide our customers a stable and secure environment. In order to do this in a repeatable manner, we have to use code to provision all infrastructure and configure all the operating systems, containers, and packages. We need to do this on a regular basis, so that we are always sure that we can recreate or change the build platform as needed. That's why the first lesson from this story is to *build infrastructure from code and rebuild it regularly to make sure it still works.*

![Failure-option-3](/assets/images/screenshots/failure-option/failure-option-3.png){: .screenshot .big}

The infrastructure that we run on occasionally has failures. Sometimes it's something we can control, like running out of disk space. But sometimes it is something we can't, such as a node losing network connectivity or having a hardware failure. Having an infrastructure failure is bad, but it can be a lot easier if you have a strategy for dealing with it.

Unfortunately, having a strategy is only effective if it actually works. You would never push code to production without testing it (right!?), and your strategy for dealing with infrastructure failure should be similar. On our project, we make a point of recreating environments from scratch regularly using automated scripts. We use this as a technique to recover from a wide variety of possible infrastructure failures. Related to this, the second lesson from this story is to *practice failing and recovering from it.*

Because we are deploying a large number of packages to multiple operating systems and container technologies, we unfortunately run into packages that disappear from the internet regularly as newer versions are released. Usually, we can just upgrade to newer versions, but occasionally that isn’t possible because of a complicated web of dependencies. If a package that you're dependent on disappears from the internet, and you don’t have your own copy stored in a safe place and a way to create an environment from that backup, you could end up in a situation where you can’t deploy your own software. This painful situation is why the third lesson from this story is *trust no one.* (Also, you need your own local backup.)

### So to recap, the three lessons from this story were:

* Build infrastructure from code and rebuild it regularly to make sure it still works.
* Practice failing and recovering from it.
* Trust no one. Make your own local backups.

Since failure is an inevitable part of building and operating software, remember that you can take steps now to make it better. You can decrease the likelihood of failure by automating everything and practicing important tasks, so there are no surprises. You can also make the failures less dangerous by having a plan and automated way to implement that plan. Having fewer failures that hurt less sounds like a victory to me.

 
Snap CI © 2017, ThoughtWorks
