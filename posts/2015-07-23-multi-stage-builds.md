---
layout: post
title:  "Deployment Pipelines in Snap CI"
description: In Snap CI, the deployment pipeline can be modeled as a series of custom build stages. Each stage consists of a set of commands which are run to accomplish a specific purpose.
date:   2015-07-23
author: Vishal Naik
index-image-url: screenshots/multi-stage-builds/manual_trigger.png
index-image-alt: 'Multi Stage Pipeline'
keywords: continuous delivery, continuous deployment, continuous integration, multi stage pipeline, multi stage builds
categories: article
---

The [Continuous Delivery book](http://martinfowler.com/books/continuousDelivery.html) talks about the concept of the Deployment Pipeline being central to CD. The deployment pipeline is essentially the path to deploy your code to production reliably.

In Snap CI, the deployment pipeline can be modeled as a series of custom build stages.
Each stage consists of a set of commands that are run to accomplish a specific purpose.

In this article, we will look at the value of multi-stage builds and how you can leverage them in Snap CI.

For this, we will look at a sample setup of a Java web application on Snap CI:

![multi stage build example]({% asset_path screenshots/multi-stage-builds/multi_stage_pipeline.png %}){: .screenshot .big}

The Build stage compiles and executes unit tests.
Smoke Tests stage starts up the web application in the build environment and runs WebDriver tests against it.
Package stage bundles the application into an archive file.
Pre-Prod stage is configured to automatically deploy the artifact to a staging environment which is used for manual QA.
Prod stage is configured to deploy to Production using a manual trigger.

There are many advantages of dividing the build process up like this.

## Stages provide high-resolution visibility into the pipeline

If your deployment pipeline is a single monolithic script, assessing severity of build failures can be time consuming and requires developers to analyze logs to know where and why something failed.

Stages provide a logical organization of the deployment pipeline.

For instance, in the example below, the Pre-Prod step has failed and is highlighted as such. Clicking on the failed stage takes me directly to the console logs of that stage without having to wade through logs from previous successful stages.


![better visibility]({% asset_path screenshots/multi-stage-builds/better_visibility.png %}){: .screenshot .big}


Model too few stages and you do not get enough visibility: too many stages will dilute it.
Instead, model stages around logical groups that give the most valuable feedback to your team.

For example, if you have categorized your functional tests into a fast feedback “Smoke Test Suite” and an exhaustive but slow “Regression Test Suite”, then having separate stages for each can be a good idea.



## With Stages, you only need to build the artifact once.

In our example, we build an artifact in the Package stage and the same artifact is used in Pre-Prod and Prod stages. This is important not only for efficient pipelines but also because changes in environments--like a compiler version, dependencies etc.--can result in different artifacts getting deployed into Pre-Prod and Prod, thus reducing confidence in the pipeline.

Snap CI allows you to export artifacts after every stage.


![artifact propagation]({% asset_path screenshots/multi-stage-builds/artifacts.png %}){: .screenshot .big}

Notice the path mentioned in the “Artifacts” section above. The artifact will be available for all subsequent stages out of the same location.

![artifact download]({% asset_path screenshots/multi-stage-builds/download_artifacts.png %}){: .screenshot .big}


Additionally, the artifacts can also be downloaded either from UI console or using an API

![artifact download]({% asset_path screenshots/multi-stage-builds/download_artifacts2.png %}){: .screenshot .big}


## Stages can be re-run independently

We can re-run a specific stage instead of re-triggering the entire build. This can be valuable when the root cause of the build failure was due to a flaky test or an external dependency like the deployment environment not being available. Re-runs can also be used on a previous build to rollback to an older version.

![re-runs]({% asset_path screenshots/multi-stage-builds/rerun.png %}){: .screenshot .big}

## Stages can be configured to run on a manual trigger

Sometimes, you don't want every commit to be deployed to Production immediately. This is useful when you want to review changes, typically after deploying to a Pre-Prod/Staging environment but before deploying to Prod.

Indeed, the ability to decide when to release to Production versus always deploying is the simple difference between Continuous Delivery and Continuous Deployment.

In this example, we have the PROD stage configured with a manual trigger.

![manual trigger]({% asset_path screenshots/multi-stage-builds/manual_trigger.png %}){: .screenshot .big}


## Better visibility through stage history

Stage History allows you to see what features will get deployed when you execute the manual trigger on the Prod stage.
This view can additionally help to identify when a particular feature was deployed so that you can answer questions like 'There is a spike in errors after 11 AM. Did we deploy any change that could have caused this?'

The Stage History view can help you determine just that and help you to immediately roll back deployments to a stable version using the 'rerun' Stage feature.

![stage history]({% asset_path screenshots/multi-stage-builds/stage_history.png %}){: .screenshot .big}

I hope you now that you've read this you have a better picture of how to leverage stages in your Snap CI pipelines. Feel free to reach out to us in case of questions or feedback.

 
Snap CI © 2017, ThoughtWorks
