---
layout: post
title:  "5 CI/CD Strategies for Faster Software Deployments and Better Automation"
description: "Here are five ways to increase speed through continuous delivery and continuous integration practices."
date:   2016-08-30
author: Suzie Prince
index-image-url: screenshots/5-ways-ci-cd-faster/snap-ci-automate-everything.jpeg
index-image-alt: '5 ways for faster software deployments'
keywords: continuous integration, deployment, automation, deployment pipelines, build pipelines, parallelism
categories: SOFTWARE-DELIVERY
---


It's been shown that teams and organizations who adopt continuous integration and continuous delivery practices significantly improve their productivity. Many teams already have healthy dev practises - well-tested code, clear business goals, collaborative teams - yet we often hear that teams still want to release sooner, deliver faster and be more efficient. Here are five ways to make the most of CI/CD practices.


### 1. FREQUENCY IS EVERYTHING

Frequency reduces difficulty. Why? Because in doing something more often, the task becomes smaller, making it easier to achieve. As ThoughtWorks' chief scientist and renowned software consultant Martin Fowler discusses [in this post](http://martinfowler.com/bliki/FrequencyReducesDifficulty.html), continuous integration is a key example of something that becomes easier and easier the more often you do it. Continuous deployment is a similar example: If you release a small amount of code to production, you have less to test, less to break and less to go wrong. And if something does go wrong, you are way more likely to understand what caused it because the changes to that environment were smaller and happened closer to the time of release, so they're easier to remember and resolve.

Also, because you are doing something more often, you become more aware of the task's cost and can find opportunities to improve your processes. You also become (hopefully!) better at doing it. Yet another nice side benefit is getting feedback faster. So while doing tasks over and over might seem counterintuitive, even painful, the result is that you become more adept and less stressed.

My advice: commit code to your repos more often, integrate branches to 'master' at least once a day, and push to prod regularly.  

### 2. AUTOMATE, AUTOMATE, AUTOMATE

![Automate everything](/assets/images/screenshots/5-ways-ci-cd-faster/snap-ci-automate-everything.jpeg){: .screenshot .big}

The drudgery of performing the same tasks over and over again is a powerful motivation to automate them. Automation is a cornerstone of strong continuous integration and continuous deployment. Automation doesn't just reduce time (see #3) and waste: automating things like deployment to staging environments, acceptance tests and cross-browser tests can increase productivity and reduce errors, allowing you to release a better quality product. In order to be more productive, take the advice of [David Rice and Aravind SV](https://www.go.cd/2016/01/25/are-you-ready-for-continuous-delivery/) by asking "Why can't this be automated?" every time anyone on the team does anything manually more than once. Here are a few examples of things we’ve seen be automated that you might not consider natural automation candidates: documentation, status updates on Twitter, installer testing and testing [chaos](http://techblog.netflix.com/2015/09/chaos-engineering-upgraded.html). And if you want help on deciding if something is worth the time to automate, check out this [XKCD](https://www.google.com/url?q=https://xkcd.com/1205/&sa=D&ust=1471369997072000&usg=AFQjCNHSGnaneL-28LCoSFJDmgiwhKv-0Q) :)

### 3. RUN TESTS IN PARALLEL

Waiting for tests to run can be very frustrating. When each test executes individually, you have to wait until they're *all* complete before you get feedback. By running multiple tests in parallel, you can run more tests at the same time, decreasing the total amount of time spent on testing and waiting for feedback.

There are a variety of tools to help you parallelize tests depending on your language and test framework. In Snap CI, we have provided some out-of-the-box test parallelism for Ruby. We use a Ruby gem that will automatically partition Ruby tests based on the number of tests you have and the number of workers you are running your builds with. Taking advantage of out-of-the-box parallelization is an easy way to get test results sooner.

### 4. HAVE A DEPLOYMENT PIPELINE

![Deployment pipeline](/assets/images/screenshots/5-ways-ci-cd-faster/snap-ci-deployment-pipeline.png){: .screenshot .big}

Deployment pipelines are another cornerstone of continuous delivery and continuous deployment that help speed up your deployments. Having a deployment pipeline means that every time you make a change to the code, integrate and build the code, you also automatically test your code on environments that are similar to production. A deployment pipeline usually has more than one stage and moves from stages that are fast-running to slower, more comprehensive stages. It often has stages to test in a development environment, a test environment, and a staging environment, but the stages you choose will depend on your team, product or organization.

One big benefit of the deployment pipeline (and why I am mentioning it here) is that by triggering your fast-running stages before your longer-running test suites, you can get feedback sooner about the viability of your code and can understand whether it is production-ready more easily as well. In addition, the highly visual layout of pipelines in tools like Snap CI makes it easy to understand the path to production, and simple for the entire team to understand where the failures are.

Once you have automated test suites a good place to start with implementing a deployment pipeline is to make deployment to any environment a fully automated process that happens on demand in minutes. By doing this you can get feedback sooner and make the path to production faster and repeatable.

### 5. UNDERSTAND PULL REQUESTS AND BRANCH BUILD STATUS

Use continuous integration on all branches and pull requests. Understanding the status of your master  is just one way to make deployment faster. If you are practicing branch-based development and using pull requests, you will need to know the status of all branches and pull requests that will merge to release. [Surveys](http://cope.eecs.oregonstate.edu/CISurvey/) have shown that pull requests that have CI build status information are more quickly merged. And the faster you merge, the faster you get to production. Snap CI goes one step further by providing [integration pipelines](https://docs.snap-ci.com/working-with-branches/integration-pipelines/) which are triggered on successful completion of the primary pipeline. The integration pipeline is triggered with the branch changes merged with the HEAD of the integration target branch.

My advice is to use CI on all branches and pull requests to get faster feedback on specific branch changes as well as feedback on what will happen when the branch is merged to master.


I hope this has been useful. To recap, my five CI/CD strategies for faster deployments and better automation are to integrate and deploy frequently, automate relentlessly, run tests in parallel, use deployment pipelines, and understand how to use CI on all branches and pull requests.

 
Snap CI © 2017, ThoughtWorks
