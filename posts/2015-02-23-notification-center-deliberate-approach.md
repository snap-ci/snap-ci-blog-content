---
layout: post
title:  "A more deliberately designed notification center"
description: Access the notification center from the "build history" page, and the ability to specify exactly what aspects of your build you wish to be notified about.
date:   2015-02-23
author: Badri Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, travis ci, campfire, hipchat, slack, ccmenu, buildreactor
categories: announcements
---


Something was wrong. We had built many different ways for people to radiate and share the statuses of their builds but there wasn't a week without folks wondering if we had GitHub build badges or Slack support. Or wonder where they could configure which emails they got from Snap. Something was obviously awry.

### A history of fragmentation

The first notification system we built in Snap notified users of build failures and fixes via email. This version of email notifications did not allow users to configure which emails Snap sent out, or where they received their emails. These issues and others were subsequently remedied.

Following this came build shields. [TravisCI](https://travis-ci.org/) initially did this incredibly clever/useful thing with build shields that could be embedded into the README of repositories. Like many great ideas, this was so simple - and to paraphrase the folks at Apple, so "inevitable" - that we couldn't but pay homage to it. We added that  functionality to Snap essentially unchanged.

Around this time, we acknowledged that because teams hung out on chat rooms, integration with the popular ones would soon be necessary. With that in mind, we put in [Campfire](https://campfirenow.com/) and [HipChat](https://www.hipchat.com/) notifications next. (Slack hadn't been released at this point)

But pretty soon, like many of you, we found our own team all in a [Slack](https://slack.com/) room. Coming seemingly out of nowhere, Slack quickly became the cool-kid-on-the-block. We needed it before anyone else and so that's how Slack integration got in.

This may seem like notifications aplenty, but we added even more. Users of older CI tools had gotten used to the XML format of the CCTray tool. In a sense, it had become the default design inspiration for a bunch of build notification tools got built. [CCMenu](http://ccmenu.org/), [BuildReactor](https://chrome.google.com/webstore/detail/buildreactor/agfdekbncfakhgofmaacjfkpbhjhpjmp?hl=en), etc, all used this format. This then became yet another notification mechanism.

Last, but not the least, we always had webhooks. Not because we wanted folks to do something specific with them, but precisely because we had no opinions on what should be done. We love tools that encourage hacking. We love the joy of serendipitous discovery and the ability to scratch an itch. Webhooks and good quality APIs are a way to do that. So that's why they went in.

While each of these decisions alone makes a lot of sense, when seen together and lacking an organization principle, they inevitably ended up getting scattered throughout the application. This caused many of you to wonder - and rightly so - which of these mechanisms we supported and how to configure them.

### From casual to deliberate

Over the last few weeks, we have spent time consolidating all these mechanisms on Snap. You can now access the new notification center from the "build history" page. In addition to this consolidation, we have introduced the ability to specify exactly what aspects of your build you wish to be notified about.

![notification center]({% asset_path screenshots/notifications/notification-center.png %}){: .screenshot .big}

We would love to hear what you think of the new consolidated notification center. Do you see all the channels you expected to see? See anything missing? Let us know in the comments below.
