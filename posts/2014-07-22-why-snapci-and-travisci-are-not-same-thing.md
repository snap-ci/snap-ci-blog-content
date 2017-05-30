---
layout: post
title:  "Why Snap-(CI) and Travis-(CI) are not the same thing."
description: What is the difference between Snap CI and other continuous delivery tools?
date:   2014-07-22
author: Marco Valtas
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, travis ci, codeship, circle ci, deployment pipeline
categories: continuous_delivery
---

As a member of the Snap team, you can be certain of two things. First, you will learn a lot about how projects get built and deployed. Second, you will, sooner or later, have to answer the following question:

> "What is the difference between Snap and [Travis](https://travis.org)
> ([CircleCI](http://circleci.com), [Codeship](https://www.codeship.io),
> [and](https://drone.io) [others](https://buildbox.io))? Why should I use
> Snap?"

## Continuous Integration (CI) and Continuous Delivery (CD)

The two terms often get mixed together in people's mind and speech. Perhaps this is due to the dependency between them. You can't practice Continuous Delivery without first having a strong discipline around Continuous Integration. However, these are two very different concepts. In short:

* Continuous Integration - the automated build and test of your software when a change is made by anyone on the team, with all work happening on a shared branch (aka master)
* Continuous Delivery - the ability to keep your software shippable at any point in time. This is usually made possible by having a rigorous set of automated tests and robust automated deployment scripts and tools.

## Snap-(CI) and Travis-(CI)? What's the difference?

There is a fair bit of similarity in the names of both tools and also in that they both allow you to build and test your software.

Building and running tests when a change happens is the basic premise of Continuous Integration. Both Snap and Travis have this feature. However, Snap augments this with the idea of a **Deployment Pipeline** as stated on Part II of the Continuous Delivery book.

What this means is while most Continuous Integration tools consider CI to be the endpoint (or maybe allow you to configure an automatic deployment), running tests is but the start of the journey in Snap. You see his when you start to compare some of the other features of Snap.

## Snap and the Deployment Pipeline

Here's an example of a simple Deployment Pipeline:

![Snap Pipeline]({% asset_path screenshots/why-snapci-and-travisci-are-not-the-same-thing/snap_ci_pipeline.png %})

The first rectangle show a set of changes that were introduced together with a link to [Github](https://github.com) for more details. The rest are the **stages** of this pipeline.

### What are stages and what are they for?

Stages are a set of logically related commands.

To confidently say that your software is production ready, you need to look it at from multiple different angles. Unit tests, code formatting, functional tests, security checks, performance tests and so on. On more advanced projects, this might also include deployment of the software to testing environments and the execution of either automatic or manual tests against these deployed testing environments.

Stages model each of these activities. Your software goes through these stages, one by one, and can be considered "good for production" when it pass all of them. On the flip side, when a stage fails, it reveals that an aspect of the software's quality has failed its test for production readiness. When a stage fails, the pipeline is stopped and the team members are notified about the failure. Since there is a logical organization of these steps, when one fails, the team can quickly identify in which set of commands the failure has happened. This results in much faster feedback than would otherwise be possible.

The ability to organize these automation steps allows you to separate different concerns when preparing your software to be deployed to production. Snap's support for deployment pipeline lets you create stages and also shows exactly how far each set of changes made it down the pipeline.

### Deployment

At the end of a series of stages you can deploy your software to production. However most development teams do not jump to production after all automated tests pass. Instead, they use an environment similar to production for further testing and verification; usually called Staging or UAT (short for user-acceptance-testing).

An important aspect here is to control what, where and when things get released. Coming back to the Continuous Delivery definition above:

* Continuous Delivery - the ability to release your software on demand, with
    high quality and low risk; most often achieved by pervasive automation.

The "on demand" part of the definition translates to a "manually triggered stage" on Snap:

![Snap - Manually Triggered Stages]({% asset_path screenshots/why-snapci-and-travisci-are-not-the-same-thing/snap_ci_manual_stage.png %})

What this means is that this stage, called "publish", won't be executed as soon as the prior stage completes successfully but only when someone manually triggers it by clicking it. In this particular example this means that any change made into the project won't be put into production automatically, but only when you want to.

You can have any number of manual stages, and between them, automatically triggered stages. This is a key part of this feature. It gives you the flexibility to create deployment automations that exactly as simple or as complex as the needs of your business and your software.

Manual stages can be triggered multiple times. This means you can deploy old versions of your application. But in order to do that robustly, it is important to configure artifacts.

### Artifacts

Each stage executes a series of commands to verify your software. Once this is done, if you want to deploy your application on to UAT or production what do you do about your compilation artifacts? If your project creates packages, binaries etc, do you have to create those again? Should you?

The answer is no. This is where artifacts help. You can ask Snap to save the contents of one or more directories in your build as an artifact directory. Snap will then save and restore the contents of this directory for each stage. The importance of this feature is explained in detail as the first practice of deployment pipelines:

> "Compile your binaries only once"
> Continuous Delivery, Chapter 5, page 113.

As result, when you have a manual stage that deploys to production, you can have it deploy the exact same binary which went through all the previous stages of this pipeline, regardless of how far back in history the pipeline was initially triggered. Recompiling, repackaging or more generally, recreating your artifacts can be problematic, as small changes in the environment could subtly impact how your software is packaged  resulting in errors that are hard to track down/diagnose. Reusing artifacts eliminates this whole class of problems.

Artifacts are also helpful for verifying and debugging. By maintaining a history of your packages; developers, quality analysts, security audit folks and others can retrieve a specific version of your software and test it knowing that it is exactly the same software as in any given environment.

### Continuous Integration

Among Continuous Delivery recommendations for Continuous Integration practices there's a highlight on branch usage (Branches, Streams, and Continuous Integration, Chapter 14, page 390).

The basic idea is, if different teams work on the same code but are split by branches, we can't assume that they are integrating these changes continuously. Even when they are, the state of the individual branches is not visible to the team at large. This lack of visibility means that potential conflicts are not highlighted early and we are merely postponing their resolution.

This, of course, doesn't mean that you shouldn't use branches; just that they should be as short lived as possible.

Snap has a solution to help out on these code integrations and highlight when two or more streams of work are diverging. When [_Automatic Branch Tracking_](http://docs.snap-ci.com/working_with_branches/automatic_branch_tracking/) and [_Integration Pipelines_](http://docs.snap-ci.com/working_with_branches/integration_pipelines/) is enabled, Snap will not only execute the pipeline on the branch but will also attempt to do a local merge of the branch with the main tree and execute the pipeline against the merged code. Like so:

![Snap - Automatic Branch Tracking]({% asset_path screenshots/why-snapci-and-travisci-are-not-the-same-thing/snap_ci_auto_branch_tracking.png %})

By the end of the pipelines you not only know that your changes pass through the pipeline but also it also will pass if you perform a merge with the main development tree. You are always aware if your changes are diverging from the main tree and in a position to take corrective action if required. Also, the whole team has visibility into the health of each branch.

### Auditing and Compliance

Finally I will bring up a topic that may not be too popular among developers but is important for IT organizations in general, Auditing and Compliance. This topic is briefly handled on Continuous Delivery, Chapter 15, page 436.

When you see that your deployment pipeline is ready for deployment. If you start the deployment, what changes will be published?

To answer this question Snap give you the "Stage History" feature, where all changes between stages can be visualized. Here's a example of some changes that will go into production if you deploy:

![Snap - Stage History]({% asset_path screenshots/why-snapci-and-travisci-are-not-the-same-thing/snap_ci_stage_history.png %})

This history allow you to check all previous changes that got deployed and which will be deployed next. In addition, it also tells you who triggered the past deployments and when - giving the traceability many teams crave.

## Conclusion

With these examples I hope I have explained what differentiates Snap from similar offerings. The name and basic features appear less similar as soon as you start to dig deeper and compare other features. The practice of Continuous Integration is a subset of Continuous Delivery. However, a tool for doing CI cannot be trivially extended to support Continuous Delivery - even if it allows you to configure a deployment at the end of your build.

This explicit focus on features that enable Continuous Delivery is the primary difference about Snap.
