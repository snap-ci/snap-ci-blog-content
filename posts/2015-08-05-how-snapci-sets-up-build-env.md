---
layout: post
title:  "How Snap CI currently sets up build environments"
description: "Part 1 of our series on 'Bringing Docker Support to Snap CI'. Here, we talk about how Snap CI currently provides build environments for user builds."
date:   2015-08-05
author: Akshay Karle
index-image-url: screenshots/snap-containers/blog-image.png
index-image-alt: 'Bringing Docker Support on Snap CI'
keywords: docker, openvz, container technologies, continuous integration, continuous delivery, lxc, linux, hosted ci
categories: article docker
---

This is Part 1 of the blog series on “Bringing Docker Support to Snap CI”. In this blog, we talk about how Snap currently provides build environments for user builds.

Snap CI is a hosted CI/CD service that allows users to setup build and deployment pipelines for their code repositories.

Users that set up builds expect their builds to start as soon as they push their code to GitHub.  In order to achieve this goal, we make sure our build machines are always prepared and ready to build as soon as we receive any build requests from GitHub. These build machines should be identical and all should include the latest languages, databases and libraries required by SnapCI's wide customer base. On top of this, users can customize their dependency versions, environment variables, install their choice of database, compiler or SDK.

## Challenges in a hosted CI server setup

* Multi-tenancy - A hosted CI solution cannot afford to run dedicated instances per customer if they want to give low price points to users.
* Isolation - Every build that runs must be completely isolated from previous or concurrent builds.
* Security - A user build should not be able to break out and affect other users in any way.
* Identical environments - Every build environment must be identical and must start from a clean state.
* Fast provisioning - We should be able to quickly provision build environments upon user request.

To fulfill these criteria, we had to leverage virtualization techniques for running user builds.

## Virtualization techniques

#### Hypervisor based virtualization

Hypervisor based virtualization technologies like [VMWare](http://www.vmware.com/), [VirtualBox](https://www.virtualbox.org/) and [Xen](http://www.xenproject.org/) that allow running a full OS on virtual hardware (guest OS), can address these challenges by running each build inside a virtual machine (VM).  However, the hypervisor layer (the layer that manages the guest OS) adds significant, prohibitive overhead to the performance.

> "While VMs excel at isolation, they add overhead when
  sharing data between guests or between the guest and hypervisor.
  Usually such sharing requires fairly expensive marshaling
  and hypercalls. In the cloud, VMs generally access
  storage through emulated block devices backed by image files;
  creating, updating, and deploying such disk images can be
  time-consuming and collections of disk images with mostly duplicate
  contents can waste storage space." \[<cite>[IBMResearchReport]</cite>\]

#### Container based virtualization
[Linux container based virtualization](https://www.linux.com/component/content/article/186-virtualization/300057-containers-vs-hypervisors-choosing-the-best-virtualization-technology-) on the other hand,  is a virtualization method where the kernel of an operating system allows multiple, isolated user-space instances. All containers share the same kernel, which means it doesn't have the same overhead as a hypervisor and is instead very efficient and fast-loading.

Since we wanted to offer a fast, Linux-based CI system to our end users, containers seemed like the way forward.

However, back in early 2012 when Snap started, our options were limited. There were only a few major container-based virtualization technologies available for Linux:

* [LXC](https://linuxcontainers.org/) - LXC was still in its infancy and development was in full swing
* [Linux-VServer](https://en.wikipedia.org/wiki/Linux-VServer) - This was stable but was running on a very old kernel then. Also, there was not very much of community support available at the time
* [OpenVZ](https://openvz.org/Main_Page) - OpenVZ had been stable for quite a few years and had most features that SnapCI needed


## Choosing OpenVZ

Back when we started Snap in 2012, we chose OpenVZ because it had already been production ready for a few years, provided isolation in all the different [namespaces](https://lwn.net/Articles/531114/) and included most of the features we were looking for. For e.g. with OpenVZ, we could provide root access within a container without being privileged on the host or other containers. This allowed users to have admin privileges required to customize their build environment on the container but at the same time secured our users against a major attack vector that other container technologies were vulnerable to at that time. \[<cite>[UserNamespaces]</cite>\]

As of today, every build on SnapCI runs in an OpenVZ container. When a build triggers, we start up a container, run the build and when the build completes we use the OpenVZ snapshots to restore the container to a clean state. This ensures that we don't leave behind any sensitive user data and the fact that all containers remain identical.

Over the past few years, we have seen a lot of excitement and improvements in container technologies -

* LXC became [production ready](https://lwn.net/Articles/587545/)
* [Docker](https://www.docker.com/) became [production ready](https://blog.docker.com/2014/06/its-here-docker-1-0/)
* [Rocket](https://github.com/coreos/rocket) was announced although not production ready

## Bringing Docker Support

Docker especially has gained significant traction in the last few months, and we’ve been getting numerous requests from our users to let them use Docker to build and test their containers as part of their build pipelines in Snap. However OpenVZ our current container technology, did not support running Docker until [a few weeks ago](https://openvz.org/Docker_inside_CT) but, we ran into problems trying run Docker inside our custom OpenVZ containers and wanted to move away from a custom and old kernel required to run OpenVZ containers.

So, after three years, we are re-visiting our choice of container technology.

LXC and Docker have improved significantly over the last few years, and we took this opportunity to explore them.

In next article in this series, we present our views about these new container technologies and our reasons for choosing the next container technology going forward for running your builds on Snap.

If you're interested in using Docker on Snap CI, [you can sign up here](https://orca.snap-ci.com/).


[IBMResearchReport]: http://domino.research.ibm.com/library/cyberdig.nsf/papers/0929052195DD819C85257D2300681E7B/$File/rc25482.pdf "Performance comparison of Virtual Machines and Linux Containers"
[UserNamespaces]: https://lwn.net/Articles/532593/ "User namespaces in Linux"
