---
layout: post
title:  "Why we decided to use LXC instead of Docker for build environments in Snap"
description: "Security aspects around using containers for build environments led to our decision choosing the container technology for provisioning build environments in Snap CI"
date:   2015-09-09
author: Akshay Karle
index-image-url: screenshots/snap-containers/blog-image.png
index-image-alt: 'Containers in Snap CI'
keywords: docker containers virtualization security lxc snap continuous delivery continuous integration
categories: article docker
---

This is part 3 of the series on “Bringing Docker Support to Snap CI”. In this article, we talk about security aspects around using containers for provisioning build machines in Snap CI.

Customization is an important aspect in a hosted CI environment. For example, users may need to install different packages, libraries, and services required by their builds that are not provided by default. In order to accommodate this, Snap CI allows users to run arbitrary commands, some of which may need to run as root.

But of course, providing root access within the container of the build environment has huge security implications for a multi-tenant, hosted build server.

## The need for user namespace in containers on multi tenant systems

In [part 1 of this series](https://blog.snap-ci.com/blog/2015/08/05/how-snapci-sets-up-build-env/), we explained that we adopted OpenVZ containers for build environments back in 2012 because:

“With OpenVZ, we could provide root access within a container without being privileged on the host or other containers. This allowed users to have admin privileges required to customize their build environment on the container but at the same time secured our users against a major attack vector that other container technologies (including LXC) were vulnerable to at that time. [See [user namespaces](https://lwn.net/Articles/532593/)]"

In short, user namespaces basically afford the ability to run processes and access filesystems as a root user in the containers which can be mapped to a non-root user on the host. If a root user on a container were to find an exploit that allowed them into the host, the user wouldn’t be root on the host machine and so wouldn’t be able to cause as much damage as a root user on the host would be able to.

But as great as it is, Docker doesn't [yet support user namespaces](https://docs.docker.com/articles/security/). The [Docker security page](https://docs.docker.com/articles/security/) mentions leveraging kernel security features like [capabilities](http://blog.siphos.be/2013/05/capabilities-a-short-intro/) to harden the host with systems like SELinux and AppArmor. These tools allow whitelisting of resources and capabilities that the container needs. There is also [grsec](https://grsecurity.net/) which has simpler ways to manage the security policies. It requires patching the kernel however. This is something we may look into the future to further harden our hosts.

This DockerCon video has a good introduction to these concepts.

{% youtube zWGFqMuEHdw 480 320 %}

While those techniques to harden the system work well for trusted applications running on self-managed servers, they are not enough for a hosted build environment that executes arbitrary code because it is not possible to whitelist all the resources a user build may need in advance. Due to these security considerations, we decided not to use Docker containers as build machines.

On the other hand, LXC now has [support for user namespaces](https://www.stgraber.org/2014/01/17/lxc-1-0-unprivileged-containers/) starting with LXC 1.0 that was released in 2014.

## We chose LXC containers for build environments

Due to these considerations, we selected [unprivileged LXC containers](https://linuxcontainers.org/lxc/getting-started/#creating-unprivileged-containers-as-a-user) for build machines on top of which users will be able to build and run Docker containers.

*If you’re interested in using Docker on Snap CI, [sign up here](https://orca.snap-ci.com/).*

 
Snap CI © 2017, ThoughtWorks
