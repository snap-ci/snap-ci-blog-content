---
layout: post
title:  "From the Snap Support Team: Troubleshooting in Snap with Sudo, Shell Scripts, and Bugs"
description: "Here are three easy troubleshooting tips for common problems on Snap CI"
date:   2016-05-05
author: Ankit Srivastava
index-image-url: screenshots/from-support-desk/from-support-desk.jpg
index-image-alt: 'tips from the Snap CI support team'
keywords: snap-ci, deploy software, continuous delivery, software delivery, debugging, snap shell, sudo, continuous integration
categories: SUPPORT
---


![From the Snap CI Support Desk](/assets/images/screenshots/from-support-desk/from-support-desk.jpg){: .screenshot .big}


As product specialists, Snap's support team gets lots of requests from customers stuck with build failures and errors. In this blog post, I would like to highlight some of the cool troubleshooting features Snap offers that can help you investigate the root cause of your failure or error.

### Full sudo access

Sometime user builds fail due to the package version, meaning the package available on Snap is not compatible with their code. Snap provides full sudo user access, so you can install any package which is supported by CentOS 6.7.

For example, let's say you're looking for a higher version of Firefox. You have Firefox 38.3.0 installed on your machine, but you can still install the required version of Firefox (say version 42.0) by uninstalling the current version of  Firefox and installing it in the archive. Here's how you'd do that using the command line.

![Full sudo access on Snap CI](/assets/images/screenshots/from-support-desk/full-sudo-access.png){: .screenshot .big}


### Snap-shell

Snap offers you the ability to diagnose a build, and making it a lot more straightforward to find out what's failing. If you’re having trouble with your build, just type snap-shell as a command in your pipeline configuration. You can also refer our [longer guide to the snap-shell feature](https://blog.snap-ci.com/blog/2014/08/11/introducing-snap-shell/). Below, I've documented how you can get started using snap-shell.

![snap-shell](/assets/images/screenshots/from-support-desk/snap-shell.png){: .screenshot .big}


### Troubleshoot a failed stage with bug icon

Sometimes you want to quickly investigate a stage failure. Changing the configuration and running a whole pipeline run can be cumbersome. Snap provides a "stage debug re-run" functionality which aims to help solve these issues. When you see the failed stage, you can click the bug icon to get more information on what's causing the failure. For more, you can watch our [walk-through video](https://blog.snap-ci.com/blog/2016/01/12/snap-shell-debugger-feature/).

![snap-shell](/assets/images/screenshots/from-support-desk/failed-stage-debugging.png){: .screenshot .big}
