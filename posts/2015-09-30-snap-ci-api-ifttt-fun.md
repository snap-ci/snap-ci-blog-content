---
layout: post
title:  Fun with Snap CI API and IFTTT Do Button
date:   2015-09-30
categories: hacks api
description: With Snap APIs, you can have some fun trying out new things, including designing your own deployment buttons from your phone.
keywords: deploy, ifttt do it button, dropbox, fswatch, api, manual stages, continuous delivery, continuous deployment, solutions
author: Marco Valtas
index-image-url: screenshots/snapapidobutton.png
index-image-alt: 'snapAPIdoButton'
---

<span style="font-style:italic">Who says you can't have fun doing Continuous Delivery? Snap user and fellow ThoughtWorker, Marco Valtas, recently designed a deploy button for pushing to production.</span>

Snap CI updated its APIs a while ago and I didn't have chance to try them. But then recently I found [IFTTT Do Button](https://ifttt.com/products/do/button) so I thought, "What about a deploy button for Snap?" Well, here it is:

{% youtube JTL4quwqKUg 480 320 %}

## Fun Goldberg Machine with @snap_ci and IFTTT

It's a sort of a [Goldberg Machine](https://en.wikipedia.org/wiki/Rube_Goldberg_machine), but fun. Basically what happens is:

* A script monitors a folder on Dropbox with fswatch
* A recipe to create a file in Dropbox is triggered through Do Button.
* The script detects the event and uses Snap-CI API to trigger a manual stage.

### Here's the script:

{% gist a997cb1f14c9a4836edb %}

For this script to work you need [fswatch](https://github.com/emcrisostomo/fswatch), [jq](http://stedolan.github.io/jq/) and [cURL](http://curl.haxx.se/). Note that I save my API key in my keychain. If you're not using a Mac, you need to figure a way to keep it a secret.

<span style="font-style:italic">This article was originally featured on [Marco Valtas' blog](http://marcovaltas.com/)</span>

 
Snap CI © 2017, ThoughtWorks
