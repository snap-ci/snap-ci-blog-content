---
layout: post
title:  "WTF is Continuous Delivery?"
description: "A beginner's guide to understanding continuous delivery"
date:   2016-05-24
author: Erin Snyder
index-image-url: screenshots/wtf/wtf-continuous-delivery.jpg
index-image-alt: 'wtf is continuous delivery'
keywords: snap-ci, deploy software, continuous delivery, software delivery, deployment
categories: WTF CONTINUOUS-DELIVERY
---


![wtf is continuous delivery?](/assets/images/screenshots/wtf/wtf-continuous-delivery.jpg){: .screenshot .big}

## What is continuous delivery? A beginner's guide

Somewhat recently I expanded my support-net (it’s like a fishing net but made of empathy and clever workarounds instead of rope) to cover more of the [ThoughtWorks Products](https://www.thoughtworks.com/products). Two of these products are Continuous Delivery and Release Management systems. Those are very big intimidating words (worthy of capitalization even) and prior to ThoughtWorks, I had no idea what any of that meant. So let’s dig in!

Some of my instructors at the coding bootcamp I attended tried to teach us how agile development works ...kind of ...but not really. None of us really practiced agile besides having daily stand-ups where we talked about blockers and successes.

In my current position, part of my job is to support an agile development tool. I really needed to learn this stuff. I figured out some of it on my own, I learned a lot from from observing my team, and now I’m doing the same with continuous delivery. Today I decided to sit down and pour out my knowledge for the benefit of anyone who might not be familiar with such things. Experienced developers can take a break: this one’s for the new kids and people like me who come from non-traditional (no CS degree here!) backgrounds. Here it is:

## First of all, how does software even get made?
When I was attending a coding bootcamp, here was my process:

* An instructor told me to write some code.
* I wrote the code.
* I ran some tests to see if the code works.
* If it didn't work, I’d try to fix it.
* Sometimes the code worked. No one knew why.+
* I submitted my code for review by my instructor, usually by issuing a pull request on GitHub, or by deploying to Heroku, or something.
* Done. Go have a cookie!

+(magic, that's why)
![continuous delivery is magic](/assets/images/screenshots/wtf/continuous-delivery-magic.jpg){: .screenshot .big}


In a workplace that practices agile development, here is your process:

* Identify a need (aka, What am I building and why am I building it?)
* Flesh out the idea (What are the specifics? Create a blueprint.)
* Design the thing (What features should it have? What user stories?)
* Code the thing (Allocate tasks to designers, developers, etc)
* Test the thing while you are coding the thing (Easier to fix it when it breaks)
* Integrate your thing into existing code base and test that too (Did your thing break someone else’s thing?)
* Deploy the thing to an environment, like staging or production (OMG it's live and everyone can see it!)
* Maintain the thing (Bug fixes, documentation, etc)
* Repeat

## What does continuous deployment do?
Some of the above steps can be automated to make the whole process faster. The faster you push a bug fix or a new feature, the happier your customers will be.

When you use continuous deployment software, here is your process:

* Think up the idea
* Flesh out the idea
* Design it
* Code it
* Each time you make a commit, it triggers a build server to package the code and sends it off for testing. You should treat any commit as if it could cause your code to be deployed to production and released to your customers, unless you do continuous delivery. More on that in a minute!
* Code is automatically deployed to different environments and tested (unit tests, functional tests, integration tests, etc). If any of the tests fail, the process stops.
* Then it’s time to maintain your code, fix some bugs and write some documentation to help your lovely support people troubleshoot the thing. :)
* Repeat

## What was that about continuous delivery?
The only difference between continuous deployment and continuous delivery is a manual trigger on the deployment stage. In continuous delivery, a human being has to push the button to deploy to an environment (staging, production, whatever).

This means that you can control your release cycle and decide what is getting pushed where and when. Deployment becomes a business decision. You can even rig up a big red button for your PM or whoever to push when it’s time to release!


![anybody can deploy with cd](/assets/images/screenshots/wtf/wtf-deploy-time.jpg){: .screenshot .big}


So that’s it! That is your very basic introduction to CD. It’s just a way to automate some of the steps that your code goes through when you’re developing software and gives you control over your release schedule. Not so intimidating after all :)
