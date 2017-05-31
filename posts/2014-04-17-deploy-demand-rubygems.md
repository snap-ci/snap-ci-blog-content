---
layout: post
title:  Releasing Ruby gems from your CI environment
description: Releasing every change you make to rubygems.org is likely not the best way to release usable chunks of functionality to your users.
date:   2014-04-17
author: Thiago Marano
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, ruby gems, automatic branch tracking, open source
categories: features announcements ruby
---

Doing Continuous Integration for Ruby gems is not a new idea. Given that, one might want to also build and release new versions of the gem to rubygems.org from your CI server(or service!). That said, releasing every change you make to rubygems.org is likely not the best way to release usable chunks of functionality to your users. A build pipeline with a manually triggered deployment stage is a much easier way to handle this case.

When setting up a build for your Ruby gem on Snap (we detect the presence of a `.gemspec` file to guess that you might be doing so), we offer to set up a deployment stage to rubygems.org that can be triggered on demand to deploy any version of the gem you have built successfully already. The video below explains better how this works:

{% youtube 1LX_5FDKigo 480 320 %}

This functionality can be combined with [Automatic Branch Tracking](/blog/2013/11/07/automatic-branch-tracking-and-integration/) to release from a named branch instead too, though in this case, you would need to configure the `Release` stage manually.

Following on the heels of [yesterday's announcement](https://bit.ly/1qAIUVJ), since most Ruby gems are Open Source or at least public, you can start using this today for free!

 
Snap CI © 2017, ThoughtWorks
