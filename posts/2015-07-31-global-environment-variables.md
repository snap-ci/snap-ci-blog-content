---
layout: post
title:  Introducing Global Environment Variables
description: Quick intro of Snap's global environment variables feature.
date:   2015-07-31
author: Fernando Junior
keywords: snap ci, environment variable, global commands, continuous integration, continuous delivery, pipeline stages, build configuration
categories: features announcements
index-image-url: screenshots/global-environment-variables/global-environment-variables-index.png
index-image-alt: 'Snap Global Environment Variables'
---

Back when we developed [pinned commands](https://docs.snap-ci.com/pipeline/#pinning-common-setup-commands), our main goal was to avoid repeated work for our users when they needed to run commands like `bundle install`, `npm install` or `gradle build` on every stage.

When it comes to environment variables, the situation was quite the same. Some variables, like service's credentials, are also eligible to be used repeatedly. To do that, one would have to go through all stages and add individual variables on each and every one.

Not anymore.

![global environment variable example]({% asset_path screenshots/global-environment-variables/global-environment-variables.png %}){: .screenshot .big}

We are glad to announce that you can now pin such variables just like any command. This has long been requested and we hope that it makes managing build configuration easier for users.

More details can be found on our [docs](https://docs.snap-ci.com/pipeline/#environment-variables%23global-environment-variables).
