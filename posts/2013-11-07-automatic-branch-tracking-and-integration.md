---
layout: post
title:  "Automatic branch tracking & integration pipelines"
description: There are well known patterns to using branches, in a disciplined manner, despite having their fair share of problems.
date:   2013-11-07
author: Badrinath Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, branch tracking, cloning, integration pipelines
categories: branching cloning integration
---

It is fairly common practice to create short-lived branches in Git to support development of new features. While feature branches do [have](http://continuousdelivery.com/2011/07/on-dvcs-continuous-integration-and-feature-branches/) their [share](http://martinfowler.com/bliki/FeatureBranch.html) of [problems](http://www.codinghorror.com/blog/2007/10/software-branching-and-parallel-universes.html), there are are also [well known patterns](http://nvie.com/posts/a-successful-git-branching-model/) to using branches, in a disciplined manner.

The key point to realize is that the problems associated from feature branches do not stem from the fact that work is done on a branch - but the fact the work is not continuously integrated into the branch designated as "mainline". The fact is while naming conventions, community accepted patterns and discipline can help ameliorate some of these problems, tool support for the discipline that mature developers have while using these conventions has been lacking.

A common method by which people attempt to insulate themselves against the evils of feature branching is by running a build against individual feature branches. This is certainly better than not having CI against your branches- but is absolutely no fix for the actual integration problem, which lies further down the line when we attempt to merge the work on the feature branch.

A newer method that is gaining some traction is running a build at the point when you attempt to merge your code into master using a pull-request and marking that pull request as being "green". The problem with this approach is that it doesn't eliminate the headache of fixing the integration problems as a last step of the development process. It also doesn't address the fact that the pull-request may get marked as "green" while the actual merging of the pull request may not happen immediately - so that information is typically out of date by the time comes to actually merge the work. Once again, it is better than *not* having this - at least you know that the patch was good at some point in time and any problems that exist could have only come after that test run- but it still doesn't do anything to bring that knowledge forward.

All the effort of doing CI at all is to bring the knowledge of integration problems forward. Bringing them to the point where they get created in the first place gives you the best shot at being able to fix them quickly, because hopefully, the delta required to fix it is smallest at that point and only grows larger from that point on. In light of this, what would an ideal CI environment for people working with feature branches look like?

Assuming good faith, and assuming that I have the discipline to merge my work with my team regularly, here is what I would like to see that would make my life easier.

When I create my feature branch, a new build should automatically be created for me in my CI system- thereby reducing the inertia required to set up a new build in the first place. When this happens, I should be able to nominate a branch as the target for integration. Subsequent changes to the feature branch from now on should not only run their own build, but also trigger an attempt to merge that work immediately with the integration target branch and run a build again.

The change running through the build tracking the feature branch will tell me if my work is healthy in isolation.

The change, merged with the integration target, and then built again, tells me if my change on the feature branch is good to integrate with the target. There are two possibilities at this point. Should I be flagged as good-to-integrate, I can (and should) do so immediately. If my change gets flagged as as something that won't merge cleanly (syntactic merge failure)- or merges, but fails tests (semantic merge failure), then my focus should be on getting to the point where those problems are fixed as soon as possible and then merging my changes.

Having the additional information about syntactic merge failures and semantic merge failures with the integration target at every step of the way is key to being able to do this painlessly.

Finally, when I finish my feature and the code is integrated into the integration target, I should be able to delete my feature branch - and the build that got set up automatically for me should get destroyed along with it.

With the changes that went out in Snap yesterday, this mode of working is supported through two features:

* automatic branch tracking
* integration pipelines

Automatic branch tracking allows you to create a pipeline in Snap anytime a branch (or a branch with a specific prefix) gets created in Github.

![automatic tracking setup]({% asset_path screenshots/integration-pipelines/auto-track-modal.png %}){: .screenshot}

Integration pipelines allow you to nominate an integration target branch for any pipeline. Any change that triggers the pipeline (called the tracking pipeline) will also trigger the operation of attempting a merge with the integration target HEAD and running the pipeline again.

![integration pipeline structure]({% asset_path screenshots/integration-pipelines/auto-track-integration-structure.png %}){: .screenshot}

In effect, each change triggers not one, but two pipeline runs. One with just that change and the other with that change merged with the HEAD of the integration target.

![integration pipelines running]({% asset_path screenshots/integration-pipelines/integration-builds-running.png %}){: .screenshot}

We hope that you find this useful.

Note: We are aware that the integration pipeline feature does not address two other possible issues. If multiple members each have a feature branch, there may be some interest in nominating multiple integration targets instead of just one.

In addition, there is definitely a case to be made for triggering the integration pipeline not just for feature branch changes - but also any time the integration target changes. We have some ideas around resolving both of these issues, but in the spirit of releasing early and often, we thought we'ed throw this out there first and see what you have to say about it before moving on to solving those problems.

 
Snap CI © 2017, ThoughtWorks
