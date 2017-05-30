---
layout: post
title:  "Failure is an Option - Choose When to Fail: Part 2 of 3"
description: "In this second part, we learn about why dark launching, or feature toggles, become important when deploying to production daily"
date:   2016-08-09
author: Jeff Norris
index-image-url: screenshots/failure-option/failure-option-21.png
index-image-alt: 'failure-lessons'
keywords: feature toggles, dark launch, deploy software, continuous deployment, software deployment, continuous delivery, devops
categories: SOFTWARE-DELIVERY AGILE CONTINUOUS-DELIVERY
---

In [part one](https://blog.snap-ci.com/blog/2016/08/02/failure-option-part-one/) of this series, I talked about lessons I learned from real-life experiences working with a large leasing application. In this second part, I'll talk about lessons I learned from [Mingle](https://www.thoughtworks.com/mingle/), the agile project management tool from ThoughtWorks. Mingle is a web application that is sold as a Software as a Service (SaaS) product, as well as an installed/on-site product. I will focus mostly on the SaaS aspects of this application in this post.

While I was on the Mingle team, we practiced trunk-based development and deployed to production daily. To enable daily deployments, we were very careful to smooth out many of the usual causes of friction in deploying an application. We had a substantial automated regression testing suite that verified that each build was ready for production. Everything up until the final release to production was automatic. The only manual intervention that is needed is pressing the button on our GoCD server to deploy a release to production.

The first takeaway from this story is that *there should be no manual steps in the deployment process other than the decision to deploy.* Manual steps are chances to introduce errors. Mingle deployments take a few minutes and we used a [blue-green deployment strategy](https://blog.snap-ci.com/blog/2015/07/01/blue-green-deployments/) so that users experience zero-downtime.

Because the Mingle team was always developing on trunk and deploying to production daily, we needed to think about keeping the app in an "always ready" state, meaning it was ready to deploy to production at any given time. This involved a simple technique called [feature toggles](http://martinfowler.com/articles/feature-toggles.html). We developed code behind a toggle and turned it on when it was completely ready. This allowed us to separate the act of deploying code from the choice to release a feature.

![Failure-option-feature-toggles](/assets/images/screenshots/failure-option/failure-option-21.png){: .screenshot .big}

Accordingly, the second takeaway is to *separate deployment from release.* Doing this lets teams notice failures or decide to remove a feature without deploying a new release. It also allowed us to [dark launch](https://blog.snap-ci.com/blog/2016/06/17/snap-ci-dark-launch-feature-toggles/) features by having them available in production and hiding the link to it behind a feature toggle. This technique gives you the ability to have certain users try out features, before sharing them with everyone else.

While working on Mingle, if something went wrong that couldn’t be fixed by toggling off a feature, we just redeployed the last successful build. In a few minutes, we were back to "normal". Being able to go back to the last build required being careful with how we deployed database changes. Typically it meant a few things: first off, we had to make changes incrementally so the changes would work with multiple versions of the code. These changes usually went in before the code that exercised them, then we'd switch over the code to use the new database version. Only after we were happy that the new code worked would we cleanup the old way that the system worked. This approach was slightly slower, but it allowed our team overall to make more progress, since we could move forward and backward more quickly.

This is an example of my third takeaway: *be aware of how your database changes affect rolling forward and backward.* This is important if you are set up for nearly instantaneous rollbacks. Rolling forward will take a lot more time, since you actually have to write and test the new code, as opposed to deploying code that is already prepared for deployment.

So what does failure look like on this project? Typically, a failure is when a feature does not do what it was intended to do. Most failures are dealt with using a feature toggle. When there is a more complicated failure, the approach is usually to redeploy while the team carefully fixes the problem to move forward later that day or the next day.

### So to recap, the three lessons from this story were:

* Eliminate manual steps in the deployment process
* Separate deployment from release using feature toggles
* Structure database changes to allow easy upgrades and rollbacks.


I hope this story has helped illustrate why these three lessons might be helpful as you work on your own software projects. In the third and final part of this series, I'll tell you some things I learned while working on Snap CI. Stay tuned.
