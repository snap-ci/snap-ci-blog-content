---
layout: post
title:  "Using Feature Toggles on Snap CI a.k.a. Dark Launching"
description: "Feature Toggles, sometimes referred to as Dark Launching, was something we eventually grew into on Snap CI. Learn about our evolution here."
date:   2016-06-17
author: Louda Peña
index-image-url: screenshots/dark-launch/dark-launch-snap-ci-badri-janakiraman.png
index-image-alt: 'dark launching on snap ci'
keywords: snap-ci, deploy software, continuous delivery, software delivery, dark launching, feature toggles, feature flags
categories: FEATURE-TOGGLES
---


![From the Snap CI Support Desk](/assets/images/screenshots/dark-launch/dark-launch-snap-ci-badri-janakiraman.png){: .screenshot .big}


Badri, a developer (and former product manager) on Snap, recently gave a talk at the HeavyBit Clubhouse in San Francisco on the evolution of feature toggle usage on Snap.

Our users use Snap CI to build and deploy their applications. So if we released a partially built feature into production that impacts our users, we could prevent lots of people from deploying their applications. On the flip side, if we only ever deployed features when every one of them was complete, we wouldn't release very often and turn our release management process into a bureaucratic nightmare. The solution was to decouple our deployment process from our release process by using feature toggles. This also came in handy to "dark launch" our newest features, such the new [Cybele stack](https://docs.snap-ci.com/the-ci-environment/stacks/).

For the full discussion, [you can watch the video at the HeavyBit website](http://www.heavybit.com/library/blog/the-evolution-of-dark-launching-on-snap-ci/).
