---
layout: post
title:  "OpenShift Deployments using Snap CI"
description: Snap has built in support for several Platforms-as-a-Services(PAAS), yet it is surprisingly easy to add any provider that accepts git push deployments.
date:   2014-11-25
author: Kelvin Nicholson
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, openshift, deployments, ssh, continuous deployment
categories: how-to deployments openshift
---

Snap has built in support for several Platforms-as-a-Services(PAAS), yet it is surprisingly easy
to add any provider that accepts git push deployments. In this example we will
add a deploy step for [OpenShift](https://www.openshift.com/), an open source PAAS from RedHat,
which includes a free tier for small projects. This example should take less than
10 minutes, depending on your experience with SSH.

#### Step 1: Add SSH Keys

You will need to add your private SSH key (i.e. `id_rsa` or `id_dsa`) to Snap, and your public
key to OpenShift (i.e. `id_rsa.pub` or `id_dsa.pub`)

You can create the keys on another machine with the `ssh-keygen` command, and
copy/paste them into them into the corresponding places. In OpenShift, this is
under Settings > Add a new key. Once open, paste in the contents of your id_rsa.pub key.

![OpenShift Key Image]({% asset_path screenshots/openshift-deploy/public-key-in-openshift.png "OpenShift Image" %}){: .screenshot .big}

In Snap, edit your configuration, navigate to your Deploy step, and look for "Secure Files" and "Add new"

![Add Files Image]({% asset_path screenshots/openshift-deploy/add-secure-file-in-snap.png "OpenShift Add Files" %}){: .screenshot .big}

Get the content of the id_rsa key you generated earlier and post it in the content box. It should look like this, with "id_rsa" as the name, "/var/go/.ssh" as the file location, "0600" as the permissions, and a real key:

![id_rsa]({% asset_path screenshots/openshift-deploy/create-secure-file-in-snap.png "OpenShift Add id_rsa" %}){: .screenshot .big}

The name and location are important, as these are the default locations that git will use.

#### Step 2: Configure Deploy

With your keys in place it is now possible to perform a git push with this single line to your Deploy stage:

```bash
git push ssh://ABCDEFGHIJK123@example.yourdomain.rhcloud.com/~/git/example.git/
```

Re-run the build, check your logs, and it should deploy. Good luck!

 
Snap CI © 2017, ThoughtWorks
