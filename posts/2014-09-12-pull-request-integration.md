---
layout: post
title:  Pull request integration
description: When someone issues a pull-request, Snap will automatically set up a pipeline, attempt to merge the request into the upstream branch, and run the pipeline.
date:   2014-09-12
author: Badri Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, pull request, integration, collaboration tool
categories: features announcements
---

We love the Github ecosystem. One of the reasons we love Github is how easy they make it to collaborate with one another - and as of writing this blog post, Github has over 6.9 million users proving that.

Pull requests are one of the features that make Github a powerful collaboration tool. We wanted Snap to participate in this collaborative process but to augment the human communication, not replace it.

Let's say I have a repository building on Snap., when someone issues a pull-request to it, Snap will automatically set up a pipeline for me, attempt to merge the pull-request into the upstream branch and finally, run the pipeline. Every additional commit made to this pull-request will trigger a new build. The history of this build stays around as long as the pull-request is open and when it is closed, automatically goes away.

<img src="/assets/images/screenshots/pull-requests/pr-list.png" class="screenshot"/>

Build statuses of the pull-requests are conveyed back to Github using the commit status API and both the submitter and I can view them in Github. In case of a failure, I can follow the details link back to the build logs on Snap.

<img src="/assets/images/screenshots/pull-requests/pr-status.png" class="screenshot"/>

Lastly, I may not be in a position to merge pull-requests as soon as they come in. The process may involve some communication and back-and-forth with the submitter and I usually do this when I have some spare time on my hands. However, during this time, the upstream branch may have moved and the merge results or test results from when the pull-request was issued may no longer be valid.

To help with this, we not only say _when_ Snap declared the pull-request healthy; we also let you re-run the build when you are ready to merge the pull-request - just to make sure that things are still kosher. Read more about this in [our documentation](http://docs.snap-ci.com/working-with-branches/pull-requests/).

What other collaboration workflows do you have that you think could be made more productive by Snap? Let me know in the comments below!

 
Snap CI © 2017, ThoughtWorks
