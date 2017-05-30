---
layout: post
title:  Introducing the Snap Shell
description: Snap offers you the ability to diagnose a build, and making it a lot more straightforward. Just type `snap-shell` as a command in your pipeline configuration.
date:   2014-08-11
author: Ketan Padegaonkar
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, snap shell, pipeline configuration, debug
categories: feature snap-shell debug
---

We have all had times when the build *"works on my machine"* but *"is red on the build machine"*. Having your build running on a managed environment, makes it even harder, because you have to keep making tweaks in order to understand what's going on.

Starting today, Snap offers you the ability to diagnose a build, and making it a lot more straightforward.

If you're having trouble with your build - just type `snap-shell` as a command in your pipeline configuration.

<img src="/assets/images/screenshots/snap-debug/build-plan-edit.png" class="screenshot-original-size"/>

When the command prompt is ready you'll see an icon on the build history page, you can now visit the logs page where you can start diagnosing.

<img src="/assets/images/screenshots/snap-debug/snap-shell-ready.png" class="screenshot-original-size"/>

The stage log page will show a command prompt, with some help on how you can use it.

<img src="/assets/images/screenshots/snap-debug/snap-shell.png" class="screenshot-original-size"/>


Here's a quick video to demonstrate this feature:

{% youtube 2E4civiDmtc 480 320 %}

Read some of our other posts on [Snap Shell](https://blog.snap-ci.com/categories/snap-shell/).
