---
layout: post
title:  "Pull Request Workflow With Snap CI"
description: "Snap CI has had pull request integration with GitHub for quite some time now. In this article, we will discuss why this is an excellent collaboration mechanism"
date:   2015-09-22
author: Vishal Naik
index-image-url: screenshots/pull-request-workflow/git-pull-request.png
index-image-alt: 'Pull Request Workflow in Snap CI'
keywords: github, pull request, repository, continuous integration tools, pipeline, developer, aws, elastic beanstalk
categories: article github
---

<span style="font-style:italic">Snap CI has had pull request integration with GitHub for quite some time now. In this article, we will revisit the workflow in more detail.</span>

Pull requests are an excellent mechanism to collaborate on projects on GitHub. With a pull request, a developer makes changes on the clone of a repo or a branch and requests for the change to be merged to the master branch — thereby enabling the repository's core committers to review the code change before accepting it.

Continuous Integration tools further augment this workflow. For e.g. Snap can be configured to execute builds for pull requests and reflect the status of the build GitHub Pull Request page. This is valuable feedback and gives confidence that the pull request can be safely merged to the master.

In this example, I am using a fork of a [sample project](https://github.com/spring-projects/spring-petclinic) from [Snap examples repos](https://github.com/snap-ci-examples).

The pipeline has been modeled with three stages: Test (run unit and integration tests), Package (build and package a web application archive) and Deploy (deploy to AWS Elastic Beanstalk).

![example pipeline]({% asset_path screenshots/pull-request-workflow/example-pipeline.png %}){: .screenshot}  

To enable pull request integration with GitHub, I click on the “Pull Request” link on the Settings page.  

![snap pull request link]({% asset_path screenshots/pull-request-workflow/pull-request-link.png %}){: .screenshot}  

This takes me to the “Pull Requests Settings” page where I can configure the build stages that need to run to validate the pull request.  

![pull request settings]({% asset_path screenshots/pull-request-workflow/pull-request-settings.png %}){: .screenshot}

In above example, I want the Test and Package stages to be run so that I know that the application is in a clean slate.

I obviously don’t want the Deploy stage to be executed for pull requests, so I delete that stage from the pipeline configuration.

![pull request stage deletion]({% asset_path screenshots/pull-request-workflow/pull-request-stage-delete.jpg %}){: .screenshot}

Apart from this, Snap also allows you to decide whether the secure files and environment variables configured for the master build should be applied to the “Pull Requests” build as well.

For pull requests from forked repos, Snap disables the option to copy configuration from master by default. This is because a malicious user can potentially expose the secure variables on your build.

Therefore, please be judicious when choosing to use the “copy configuration” option.

![pull request copy config]({% asset_path screenshots/pull-request-workflow/pull-request-copy-config.jpg %}){: .screenshot}

Let's see what happens when I submit a pull request from GitHub. To do this, I first fork the example project.  


![create github fork]({% asset_path screenshots/pull-request-workflow/github-fork.jpg %}){: .screenshot}

After pushing my changes on the fork, I am now ready to submit a pull request.


![github pull request link]({% asset_path screenshots/pull-request-workflow/github-pull-request.jpg %}){: .screenshot}

![create pull request]({% asset_path screenshots/pull-request-workflow/create-pull-request.jpg %}){: .screenshot}

After I create a pull request, note that GitHub starts showing information about pending checks — with a link to the Snap CI build for the pull request.


![pull request status]({% asset_path screenshots/pull-request-workflow/pull-request-checks.jpg %}){: .screenshot}  

Clicking on the "Detail" link takes me directly to Snap's pull request build page.  


![pull request build page]({% asset_path screenshots/pull-request-workflow/pull-request-build.jpg %}){: .screenshot}

Note that the Pull Request pipeline can be re-run at any point. This is handy if the master has moved ahead before you merged the pull request and you want to re-validate the build against the current master revision.

Once the build completes, GitHub reflects the status of the build on the pull request page.


![pull request ready to merge]({% asset_path screenshots/pull-request-workflow/pull-request-ready-to-merge.jpg %}){: .screenshot}

I now know that the pull request can be safely merged to the master!

<span style="font-style:italic">This article is Part 1 in a series from our upcoming e-Book "CD with Snap CI"</span>

 
Snap CI © 2017, ThoughtWorks
