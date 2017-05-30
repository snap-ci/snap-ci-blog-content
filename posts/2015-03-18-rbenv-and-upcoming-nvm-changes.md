---
layout: post
title:  "rbenv and nvm changes"
description: Snap CI will use rbenv to install rubies on your build machines.
date:   2015-03-18
author: Ketan Padegaonkar
keywords: sudo -l -u, sudo -1, ci tools, ruby, node, rbenv, nvm, snap ci
categories: announcements
index-image-url: screenshots/rbenv-nvm-changes/blog-image.png
index-image-alt: 'rbenv install commands on terminal'
---

In the past users have had to wait a few days after new versions of ruby, node and other languages while we deployed them to our build machines. This is because the build machines that Snap offers came pre-installed with a set of packages.

This was always an inconvenience for some of our users while they waited a few days for the latest and greatest languages and tools to become available. This was also an added disruption for the Snap team while some team members dropped what they were doing to re-build build machine images with the latest languages, tested them thoroughly and deployed them over for our end users.

Starting today we would like to announce that Snap will use [rbenv](https://github.com/sstephenson/rbenv) to install rubies on your build machines.

## How will this impact users?

This will improve the experience for users who are used to using rbenv/rvm/nvm on their local development machines. Users will no longer need to use `sudo` to manage packages anymore.

For users who have any `sudo [gem|npm]` commands in their tasks, we have already migrated those tasks to remove the `sudo` prefix on them.

We've removed bower and grunt as part of the node installations since most users need different versions of these tools. They can be installed using `npm install bower` and `npm install grunt`.

## What will happen when new ruby or node versions are released and available?

As and when new versions are released and available, they will automatically be available to Snap users. If a binary is not available, then you may get in touch with us, and we'll make it available ASAP.

If you have any comments or tips to help us improve, let us know by dropping a comment below or [contact us]({{ site.link.contact_us }}) if you have any questions.
