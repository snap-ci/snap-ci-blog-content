---
layout: post
title:  "Using Chef in a Blue-Green World"
description: Most applications these days have some form of deployment automation which is often implemented using an Infrastructure as Code tool. So what role do these tools play in a blue-green world?
index-image-url: screenshots/chef-blue-green/chef-blue.jpg
index-image-alt: 'chec-blue-green'
date:   2016-08-23
author: Badri Janakiraman
keywords: continuous deployment, feature toggles, infrastructure as code, chef, blue green deployments, software deployment, feature toggles
categories: SOFTWARE-DELIVERY CONTINUOUS-DEPLOYMENT
---

A lot has been written about [how to design](http://apiux.com/2014/09/05/api-design-sustainability/) [REST APIs](http://martinfowler.com/bliki/ParallelChange.html) and evolve [database](http://www.grahambrooks.com/continuous%20delivery/continuous%20deployment/zero%20down-time/2013/08/29/zero-down-time-relational-databases.html) [migrations](http://martinfowler.com/articles/evodb.html) in a world with [blue-green deployments](http://martinfowler.com/bliki/BlueGreenDeployment.html).

Most applications these days have some form of deployment automation which is often implemented using an [Infrastructure as Code](https://info.thoughtworks.com/Infrastructure-as-Code-Kief-Morris.html) tool such as Chef, Ansible, Salt, Puppet, CFEngine or some such. So what role do these tools play in a blue-green world?

The good thing is, to a first approximation, we can always take lessons learned from the database and service design spaces and try and apply them here. What we have learned here on the Snap team recently is that [parallel change](http://martinfowler.com/bliki/ParallelChange.html) is a pattern that works quite well in this context too.

![Chef-blue-green](/assets/images/bg-chef/bg-chef.png){: .screenshot .big}

##### An example: Snap caching

Every day or two, when we deploy a new release of Snap into production, we end up rebuilding our machines (see [Cattle not Pets](https://blog.engineyard.com/2014/pets-vs-cattle)). We started doing this back in 2013 or early 2014. Back then, [Amazon AWS EBS](https://aws.amazon.com/ebs/) did not have [SSD-backed storage](https://aws.amazon.com/blogs/aws/new-ssd-backed-elastic-block-storage/) as an option. This was a dilemma: we wanted to provide the fastest possible disk for our build servers, but the fastest SSD drives were only available as an [ephmeral/instance storage](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) option.

[Instance storage](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html), as the name implies, gets blown away every time the instance is shut down. This meant that using this storage option gave us runtime speed, but had the unfortunate side-effect that we ended up blowing away the build cache every time we rebuilt the machines. As we started deploying more frequently, our user's build caches would get blown away equally frequently, resulting in unpredictable slowdowns in the resulting build processes. So to work around this, we needed to start storing the build caches in a more durable way. Given the announcement of SSD-backed EBS volumes, we thought we could explore that option. But then, very recently, Amazon announced the [EFS service](https://aws.amazon.com/efs/) which gives NFS volumes as a resource.

##### Getting the cache working on EFS

As a part of our spike to improve the Snap's caching, we're now working to get builds running with EFS-persisted build caches. The build containers themselves would be on SSD-backed EBS volumes, but storing the build cache - with things such as downloaded files, configuration information, etc. - would be on an EFS volume. Since this feature would be built over a period of time, we needed a way to roll it out incrementally.

###### Expand

A first step toward making the parallel change work was changing our infrastructure scripts to provision the EFS volume and attach it to our EC2 instances. The Chef scripts that follow mount the EFS volume, if present, to a known location, as a sibling of the old instance storage mount.

This is equivalent to an "expand" operation, as the footprint of devices and mounts available on our build machines has changed. All old devices and mount points are still there and working as they always have: we just have the new devices provisioned and mounted.

At this point, we are ready to add a feature toggle (an ops toggle as [described here](http://martinfowler.com/articles/feature-toggles.html#CategoriesOfToggles)) to our application. This would be a toggle that switches the application between using the old instance storage and the new EFS storage. This allows us to run the application with the toggle enabled in our smoke and staging environments. When it's time to deploy, we can also enable this toggle for individual users. ( *hint hint* ;)

###### Contract

Once we've verified the new caching mechanism works as expected, we can destroy it. This process would involve the following steps. Each of these would be followed by a deployment.
* toggling on the EFS volume globally in production
* deleting the feature toggle to enable the usage of the new cache
* changing our Chef cookbooks to not format or mount the attached instance storage
* changing the AWS infrastructure provisioning code to not provision the instance storage

Once these changes are made, the footprint of the infrastructure on the machine will contract. There are now fewer devices and mount points. However, for a while in between, we will have both storage mechanisms present and running and the application will be capable of using either one on demand.

##### Summary

The combination of parallel change and feature toggles is powerful and enables us to move forward in small, incremental steps while still making pretty invasive changes to how Snap works. We hope that this has given you some insight into how infrastructure changes happen on Snap. However, far more importantly, we hope that you find the upcoming changes to the Snap cache useful. After all, all the clever engineering tricks are not worth much if they don't result in a more pleasant build and deploy experience for our users. We hope this is one more step toward making that happen!
