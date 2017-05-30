---
layout: post
title:  Announcing support for NoSQL datastores!
description: We're excited to announce that we're adding support for 3 of the most requested NoSQL datastores; CouchDB, MongoDB and Redis generally available for all builds.
date:   2013-09-12
author: Ketan Padegaonkar
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, NoSQL, CouchDB, MongoDB, Redis, datastores
categories: announcements features
---

We're excited to announce that we're adding support for 3 of the most requested NoSQL datastores. For the last few months, these have been some of the most common requests from our users.

To that end we are making CouchDB, MongoDB and Redis generally available for all builds. You may access these databases over on their default ports. We have disabled authentication, so your builds should be able to connect to them unauthenticated. As part of our "builds should start from a clean slate" philosophy these databases will be cleaned out before every build.

You can read more about this in [our documentation]({{ site.link.docs }}databases/). And as always, [contact us]({{ site.link.contact_us }}) if there's a feature you'd like to see on Snap.
