---
layout: post
title:  Snap "Heartbleed" Security Advisory
description: The various partners that Snap works with (Amazon AWS, Heroku, Github etc) have taken steps to contain the impact of the Heartbleed vulnerability.
date:   2014-04-10
author: Badrinath Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, heartbleed, openssl, heroku, aws
categories: security openssl
---

The announcement of the the [CVE-2014-0160](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0160) commonly known as the [Heartbleed](http://heartbleed.com/) bug in OpenSSL was a cause for concern among many. The various ecosystem partners that Snap integrates with or automates (Amazon AWS, Heroku, Github and others) have taken steps to contain the impact of the vulnerability. The Snap team too has taken the following steps to ensure that your data and our servers are secure.


## Actions taken on our servers

* We have patched all servers with the latest version of OpenSSL to ensure that our servers & services running on them are not vulnerable.
* We have updated the SSL certificates used at [snap-ci.com](https://snap-ci.com). You should be able to see this the next time you login.
* We have updated all keys and passwords internally used by the few members of the Snap team that have access to the production environment.
* We have recreated the OAuth keys we use to access GitHub & Heroku.

While we have no reason to believe that any data was compromised, we believed it was better to err on the side of caution and hence there are a few things you will have to do the next time you login to Snap.


# Actions taken to secure your data

* We have deleted all user sessions. This will forcibly log you out of Snap. You will have to login the next time you access the service.
* We have revoked all OAuth tokens for GitHub. This will mean that some Snap features (such as Integrated Branch Builds and Automatic Branch Tracking among others) will not work unless the person who set up the build logs into Snap again, providing Snap a new OAuth token. We recommend that everyone who set up a build on Snap do this so that all features continue to work as expected.
* We have revoked all OAuth tokens for Heroku: If you had a Heroku deployment configured, you will need to edit your build plan and re-configure it with new Heroku OAuth credentials. The process should be simple and similar to the process you followed when you created the stage for the first time. If you had other projects using these Heroku credentials, you will need to switch those deployments to use the new token as well.
* In addition to these steps, if you had any sensitive data stored on Snap (including API keys for external services not managed by Snap), you may wish to change those.


We believe we have taken all necessary precautions to get Snap secure for now. If you have any further questions or concerns, please do not hesitate to [get in touch with us](https://snap-ci.com/contact-us). We would love to help out in any way we can.


Stay safe!

 
Snap CI © 2017, ThoughtWorks
