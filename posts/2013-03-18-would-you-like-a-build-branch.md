---
layout: post
title:  "Would you like a side of build with that branch?"
description: Snap CI can clone an existing build pipeline, but point it at a different source tree/branch. Within minutes, your new patch can be tested, integrated and pushed out to production
date:   2013-03-18
author: Badrinath Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github
categories: feature cloning
---

It is a rare application that doesn't ever need a hot-fix to a deployed version. If to err is human, I prove I am human many times a day and sometimes, these get into production. Chances are, yours might too.

When a bug is discovered, it is often a little while into a new release with work having already been started on new features. Patching the problem in production is often a case of creating a branch from the last commit that was deployed, writing tests to expose the bug, fixing the bug, moving the tests and the fix to the branch using some functionality present in your version control tool (transplant, cherry pick, patch etc). In some cases, you may choose to move just the tests and re-implement the fix itself.

However, all these changes happen on a new branch. The code changes, along with their tests, need to be integrated and tested before you can deploy them. This is, exactly the sort of high-stress situation when you do not want to spend time wondering how to set up a branch build - or worse still, find the whole process too painful and forgo a branch build altogether.

To help with this, Snap now has the ability to clone an existing build pipeline, but point it at a different source tree/branch. Within minutes, your new patch can be tested, integrated and pushed out to production.

{% youtube ZmubQW03K1U 480 320 %}

 
Snap CI © 2017, ThoughtWorks
