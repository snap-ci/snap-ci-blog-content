---
layout: post
title:  "From the Snap Support Team: Common NPM issues"
description: "Here are three easy troubleshooting tips for common problems on Snap"
date:   2016-06-21
author: Ankit Srivastava
index-image-url: screenshots/from-support-desk/from-support-desk.jpg
index-image-alt: 'tips from the Snap CI support team'
keywords: snap-ci, deploy software, continuous delivery, software delivery, debugging, snap shell, sudo, continuous integration
categories: SUPPORT
---


![From the Snap CI Support Desk](/assets/images/screenshots/from-support-desk/from-support-desk.jpg){: .screenshot .big}


As product specialists, Snap's support team gets lots of requests from customers stuck with build failures and errors. One of the most common is an npm error. If you get an npm error for your build, it could be because of the node version, or because of open GitHub issues with npm. Sometimes builds work on your local machine, but doesn’t work on Snap.

To figure out what's gone wrong, we usually start by traversing the complete build log and look for the npm error. Once we get that, we try to replicate it on our instance and determine if there are any related open npm issues filed on GitHub. I would like share one such case (below) where the node project was failing due some dependency.

#### Here's the log:

```
node-pre-gyp ERR! node -v v4.2.1
node-pre-gyp ERR! node-pre-gyp -v v0.6.4
node-pre-gyp ERR! not ok
Failed to execute '/opt/local/nvm/versions/node/v4.2.1/bin/node 
/var/snap-ci/repo/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js clean' (1)
npm ERR! Linux 2.6.32-042stab105.14
npm ERR! argv "/opt/local/nvm/versions/node/v4.2.1/bin/node" 
"/opt/local/nvm/versions/node/v4.2.1/bin/npm" "install"
npm ERR! node v4.2.1
npm ERR! npm  v2.14.7
npm ERR! code ELIFECYCLE

npm ERR! usb@1.1.0 install: `node-pre-gyp install --fallback-to-build`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the usb@1.1.0 install script 'node-pre-gyp install --fallback-to-build'.
npm ERR! This is most likely a problem with the usb package,
npm ERR! not with npm itself.
```

### Here is the complete summary of the problem:

We tried to replicate the failure scenario by forking the project and got the same error using node 4.2.1. We did some research and found [this open issue](https://github.com/nonolith/node-usb/issues/103) on GitHub which says that a higher version of GCC / libstdc++ is required that supports C++11.

#### After upgrading the GCC, we ran into another error mentioned below:

```
../libusb/libusb/os/linux_udev.c:40:21: fatal error: libudev.h: No such file or directory 
#include <libudev.h> ^ compilation terminated. make: *** 
[Release/obj.target/libusb/libusb/libusb/os/linux_udev.o] Error 1 make: Leaving directory 
`/var/snap-ci/repo/node_modules/usb/build' gyp ERR! build error gyp ERR! stack Error: `make` failed with 
exit code: 2 gyp ERR! stack at ChildProcess.onExit
(/var/snap-ci/repo/node_modules/npm/node_modules/node-gyp/lib/build.js:276:23) gyp ERR! stack 
at emitTwo (events.js:100:13) gyp ERR! stack at ChildProcess.emit (events.js:185:7) gyp ERR! 
stack at Process.ChildProcess._handle.onexit (internal/child_process.js:200:12) gyp ERR! 
System Linux 2.6.32-042stab105.14
```

Finally, we were able to make it work with node version 5.7 and installing libudev package.

#### Here are the commands we used:

```
sudo wget http://people.centos.org/tru/devtools-2/devtools-2.repo -O /etc/yum.repos.d/devtools-2.repo
sudo yum install devtoolset-2-gcc-c++ -y
g++ --version
source /opt/rh/devtoolset-2/enable
g++ --version
sudo rpm -Uvh 
ftp://ftp.muug.mb.ca/mirror/centos/6.7/updates/x86_64/Packages/libudev-devel-147-2.63.el6_7.1.x86_64.rpm
npm install -g grunt-cli
npm install
grunt
```

We hope this will be helpful to you as you navigate running builds from Snap. [Read more](https://blog.snap-ci.com/categories/support/) articles from our support desk.
