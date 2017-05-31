---
layout: post
title:  "Faster builds with bigger sized workers"
description: With Snap CI, you get to run your builds on workers as large as you might need.
date:   2015-02-13
author: Ketan Padegaonkar
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, workers, build processes
categories: announcements feature workers
---

Your builds on Snap run on workers. These standard sized workers come with 2GB of memory and 2 processing cores. This is sufficient for most builds. Some users, however, need additional memory or cpu cores for some of their tests. Typically these are for tests that require using multiple browsers, larger JVMs and Android emulators. Other suites and build processes might benefit from the additional compute horsepower that comes with additional processors.

Today's announcement is a follow up to our story about using multiple workers to get [faster feedback]({% post_url 2014-12-19-faster-builds-multiple-workers %}). Starting today, you get to run your builds on workers as large as you might need.

The way this works is when you purchase multiple workers, you can either split your workload across them or put them together to get double, quadruple (or even larger!) workers. This will apply for users using the free trial or on any plan that allocates more than one worker. Of our standard plans, the small team plan and small business plans will have this ability - though we also sell additional workers to meet your needs.

You can read more about this feature in our [documentation]({{ site.link.docs }}workers/configuring-workers/).

![bigger workers]({% asset_path screenshots/bigger-workers/bigger-worker-configuation.png %}){: .screenshot .big}

As always, we would love to hear if you have any comments or tips to make your builds even better. Leave your comments below or [contact us]({{ site.link.contact_us }}) if you have any questions.

 
Snap CI © 2017, ThoughtWorks
