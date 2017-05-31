---
layout: post
title:  "From the Snap Support Team: Package Upgrades"
description: "Here are three easy troubleshooting tips for common problems on Snap"
date:   2016-05-17
author: Ankit Srivastava
index-image-url: screenshots/from-support-desk/from-support-desk.jpg
index-image-alt: 'tips from the Snap CI support team'
keywords: snap-ci, deploy software, continuous delivery, software delivery, debugging, snap shell, sudo, continuous integration, package upgrades
categories: SUPPORT
---


![From the Snap CI Support Desk](/assets/images/screenshots/from-support-desk/from-support-desk.jpg){: .screenshot .big}


At the Snap support desk, we commonly get requests related to package versions. It could be a package of a language (such as Ruby, PHP, etc.), a browser (Firefox, ChromeDriver, etc) or it could be database-related. As [mentioned earlier](https://blog.snap-ci.com/blog/2016/05/05/snap-support-troubleshooting-tips/), Snap gives you full "sudo" access through which you can get any package version compatible with our OS.

To work through an example, let’s see how we can get a higher, OS-compatible version of MongoDB. Currently, most of our users are using v2.4 and there are some upgrades in higher versions which are no longer supported in v2.4. Because of this, we don't want to break users' build and so keep v2.4 for now.

### Step 1 : Create a [secure file](https://docs.snap-ci.com/pipeline/#secure-files) mongodb.repo and add below content to the file.

```properties
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=0
enabled=1
```

![Create a secure MongoDB file](/assets/images/screenshots/from-support-desk/create-mongodb-secure-file.png){: .screenshot .big}

### Step 2 : Add below command to install Mongodb and start it.

```bash
sudo mv mongodb.repo /etc/yum.repos.d/
sudo yum install -y mongodb-org
mongod --version
```

![Add commond to install MongoDB](/assets/images/screenshots/from-support-desk/install-mongodb-commands.png){: .screenshot .big}


That's it! You've done it. If not, you can always [contact us](mailto:support@snap-ci.com) for help.

Read more from our [From the Snap Support Team](https://blog.snap-ci.com/categories/support/) series.

 
Snap CI © 2017, ThoughtWorks
