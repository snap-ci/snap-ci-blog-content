---
layout: post
title:  "Flipping the Switch on Jenkins"
description: ThoughtWorks said "HOLD" regarding Jenkins as a deployment pipeline on their Tech Radar earlier this year. We think that Snap CI is a great alternative to Jenkins. But why?
index-image-url: screenshots/snap-vs-jenkins/snap-ci-vs-jenkins.jpg
index-image-alt: 'jenkins is not a continuous deployment tool'
date:   2016-10-10
author: Louda Peña
keywords: jenkins, software deployment,continuous integration, build pipelines, jenkins pipelines
categories: SOFTWARE-DELIVERY TOOLS
---

ThoughtWorks said "HOLD" regarding Jenkins as a deployment pipeline on their [Tech Radar](https://www.thoughtworks.com/radar/tools) earlier this year. We think that Snap CI is a great alternative to Jenkins. But why?

![thoughtworks tech radar hold on jenkins](/assets/images/screenshots/snap-vs-jenkins/thoughtworks-tech-radar-jenkins-hold.png){: .screenshot .small}

Snap’s Product Manager, Suzie Prince, [wrote a great article](https://blog.snap-ci.com/blog/2016/09/13/snap-ci-different-better-than-jenkins/) recently outlining a handful of reasons why you should consider Snap over Jenkins. Of course, there are always appropriate use cases for every tool, including Jenkins, but you should use the best fit for your needs.

Jenkins has historically been the “go-to” tool for CI, grandfathered into new projects because it seems to be the easy solution. The plug-in ecosystem is their shining beacon in the dark night of CI tools, but once you flip on the switch, you’ll find a massive tangle that’s nearly impossible to manage.

At some point in your deployment process, you’re going to need more. It doesn’t take long to realize the benefits of an integrated deployment pipeline. It’s the foundation of [continuous delivery](http://martinfowler.com/bliki/ContinuousDelivery.html), and requires a tool that is built with this in mind. Using a plugin for something that should be inherent is the equivalent of putting a Honda Accord engine into a Ferrari: it will fit, it may even run for a short time, but I guarantee you’ll spend endless hours getting it to run properly or at all on a regular basis. If you practice CI, you might understand the problem here: [speed](https://blog.snap-ci.com/blog/2016/07/26/continuous-delivery-integration-devops-research/) and reliability are big requirements for any dev doing CI/CD.

Native deployment pipelines are also necessary to create a complete and vital experience for the software engineer. ThoughtWorks prioritizes not just making excellent software, but industry changing software - that's why Snap has a deployment pipeline built in and not created from a tangled mess of plugins.

So why not try Snap over Jenkins and see why ThoughtWorks (and us at Snap!) says "Hold" to Jenkins as a deployment pipeline.
