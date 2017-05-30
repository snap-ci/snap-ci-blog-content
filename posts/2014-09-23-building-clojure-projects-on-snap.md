---
layout: post
title:  Building Clojure projects on Snap
description: Given how much we love the language, and how many people have asked us for it, we thought it best to document how to get your Clojure projects building on Snap.
date:   2014-09-23
author: Badri Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, clojure, midje
categories: how-to java clojure
---

Guy Steele once joked that about the Java language that

[we were not out to win over the Lisp programmers; we were after the C++ programmers. We managed to drag a lot of them about halfway to Lisp](http://people.csail.mit.edu/gregs/ll1-discuss-archive-html/msg04045.html).

Clojure might well be the completion of that journey.

We have had many people ask us about getting their Clojure builds running on Snap. Many others have just hacked around and found that while we don't make a song and dance about it, it was possible to build your Clojure projects on Snap. Now we are not a song-and-dance kind of people (well, not all of us anyway ;) However, given how much we love the language, and based on how many people have asked us the question, we thought it best to document how to get your Clojure projects building on Snap.

As an example in this case, I will pick the popular Midje testing library and see how to build it on Snap. I have [forked it under my account](https://github.com/badrij/Midje) in Github.

Click on the "+ Repository" link and pick a repository. In this case, I am going to pick my fork of the Midje repository.

<img src="/assets/images/screenshots/clojure/02-select.png" class="screenshot"/>

Snap realizes that this project is already configured with a build on Travis, and so picks up the command you specified in your travis.yml file.

<img src="/assets/images/screenshots/clojure/03-default.png" class="screenshot"/>

In this case, we need a small change due to two things

* Our lein executable is just called lein- not lein2
* We do not support the travis syntax for testing against multiple versions of the language

Once the changes are made, your build should look something like this.

<img src="/assets/images/screenshots/clojure/04-configure.png" class="screenshot"/>

I have also gone and selected the version of Java I want to use explicitly. 1.8 in this case.

Having made these changes, I now save the build configuration. Snap kicks off the first build and you can watch the logs tail in your browser.

<img src="/assets/images/screenshots/clojure/06-running.png" class="screenshot"/>

In a minute or two, the build completes and you can see the test results.

<img src="/assets/images/screenshots/clojure/07-passed.png" class="screenshot"/>

Of course, this is just the start. You can use Snap to extend your build in a lot of different directions from here. You can pack and release the jars. If you were building an application, you can configure deployments following the initial stage. See the [What's next section](http://docs.snap-ci.com/getting-started/#whats-next) of our documentation for details on some of these!

Note: The first build usually runs slightly longer as it needs to download all dependent jars from either maven-central or clojars. These jars are automatically cached and subsequent runs will run much faster!
