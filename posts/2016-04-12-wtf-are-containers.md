---
layout: post
title:  "WTF are Containers?"
description: "“Container” is such a vague word. It’s an even vaguer concept."
date:   2016-04-12
author: Erin Snyder
index-image-url: screenshots/wtf/wtf-is-a-container.jpg
index-image-alt: 'what are containers'
keywords: snap-ci, docker, containers, container technology, microservices
categories: WTF DOCKER
---


## “Container” is such a vague word. It’s an even vaguer concept.

Basically, a container is something which contains things.

Helpful, right?

![WTF is a Container](/assets/images/screenshots/wtf/wtf-is-a-container.jpg){: .screenshot .big}

Container in the context of programming can mean multiple things. For one, it’s a collection of elements, such as an array or a hash.

In the context of Docker (which is an open source tool for packaging applications inside containers), containers are like slimmed-down virtual machines that can share operating system kernels (not unlike in popcorn, the kernel is the central core of the operating system and gives a Linux or Windows machine its charming personality and sometimes gets stuck in your teeth) and sometimes libraries.

While I’m trying to grasp the concept of a container, I’m going to think of it as a machine. I have a laptop sitting in front of me. It runs an operating system. I also have access to a virtual machine running Windows.

For some reason, even though my Windows VM is accessed via my laptop, it makes perfect sense to me that it’s a whole separate machine with separate files, separate OS, separate everything. For some reason, I get it. That makes sense.

So for containers, I just need to think of them like virtual machines except they can share certain things, like operating system kernels.

![Virtual Machines versus Containers](/assets/images/screenshots/wtf/docker-virtual-machines-container.png){: .screenshot .big}

## What are the benefits of using containers?

Containers allow you to wrap your software up in a neat little package with everything it needs to run, which means your code is supposed to run the same way no matter what environment it’s running in. They’re also supposed to be easy to spin up as you need them, for scalability.

## What is Docker and why is it so popular?

Docker automates the deployment of an app as a container, which makes containers easier to use. You can build-run-test-deploy your app inside a container. They’re also open-source, and developers absolutely wet themselves over OSS. Apparently there are some security concerns with using containers, so that’s something to be aware of.

So that’s my understanding of containers right now. I feel like I’ve barely scratched the surface, but as my job is requiring me to understand more about DevOps (specifically, I need to support CI/CD products), I imagine this won’t be the last time I work with and try to understand containers.

 
Snap CI © 2017, ThoughtWorks
