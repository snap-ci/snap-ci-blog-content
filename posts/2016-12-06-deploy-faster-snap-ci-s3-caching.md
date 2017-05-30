---
layout: post
title:  "The challenge to go faster: Ways we make this happen with Snap CI"
description: Caching is a very common way to speed up builds by storing local files and dependencies that are needed for your builds instead of getting them from their respective mirrors on the internet each time one runs a pipeline.
index-image-url: screenshots/s3-caching/caching.jpg
index-image-alt: speeding up your build times
date:   2016-12-06
author: Suzie Prince
keywords: DEPLOYMENT
---

Because we know you want to go faster we are always looking for ways to improve the speed of your builds. Caching is a very common way to speed up builds by storing local files and dependencies that are needed for your builds instead of getting them from their respective mirrors on the internet each time one runs a pipeline. Snap CI has had caching for a while but it was largely an ephemeral cache that was either removed by a deployment or when the storage got too full. Therefore, although it was faster than without a cache, the experience was variable - being fast sometimes and slow others - leading to an inconsistent and sometimes unexpected experience.


As a way to provide stable “fastness” and consistency in our builds time we wanted to move to a persistent storage mechanism whereby our cache would be longer lived. After spiking a number of solutions we decided to move our caches to AWS S3 storage and began to work on the solution. However, during design and implementation we had two questions we wanted to answer.


* Would cache download times at the start of a pipeline run be increased?
* Would cache upload times at the end of a pipeline run be increased?


If both were true, the desire to have speedier builds may not have been met by our new implementation. Keeping business goals, and not implementation in mind, we needed to know if builds times were improved or not to consider our new implementation a success.


In order to answer these questions we chose to use feature toggles and canary releasing to try the new caching implementation in production with a small set of real builds. This allow us to try our new solution with real builds in a real production in environment while still running our old solutions and avoiding disruption if our new implementation did not prove successful.


Answering “Would cache download times at the start of a pipeline run be increased?”


We implemented the solution and then rolled it out to our own internal builds and accessed the stage durations. These times were not increased using the new implementation.


![decreased build times with caching](/assets/images/screenshots/s3-caching/snap-s3-caching-improvement.png){: .screenshot .big}


We then continued to roll this out to more and more builds and observed improved stage durations for most users. We also informed a few users that they had been experiencing this new caching and asked for their feedback about the experience. The general consensus was “stable and quick“ which is obviously what we wanted to hear.


Based on this mix of qualitative and quantitative data our conclusion to “Would cache download times at the start of a pipeline run be increased?” was no.


Answering “Would cache upload times at the end of a pipeline run be increased?”


We had a much stronger sense that we would see a negative impact here just because the nature of S3 storage is not like copying to the ECS file system storage. However, we wanted to understand what the extent of the change would be. Basically, would we lose any benefit of moving to persistent storage by uploading to S3?


![caching was a business decision](/assets/images/screenshots/s3-caching/caching.jpg){: .screenshot .big}

In this case we found that pipeline runs were increased by ~2-20 seconds due to the upload of cache at the end of the run pipeline. In combination with qualitative data we received user feedback which was generally that “no difference” was noted at the end of the pipelines after we made the change.


Our conclusion to #2 then was that yes, cache upload times at the end of a pipeline run were increased in absolute terms but they were not in relative terms significantly higher. Compared to the average pipeline run the increase was very minimal and didn’t outweigh the other speed and stability improvements we had seen.


Given these conclusions we rolled out the changes to the complete production stack. We would not have been able to achieve this outcome and confidence that our changes would have the desired business goal without implementing feature toggles, continuous delivering our solution to production, gathering data about the impact of the change and using canary releasing techniques to test with a small number of users.


I hope this example has shown you how these techniques can help prove out product management and business decisions as well as provide feedback into the technical solutions product teams make.


Go forth, deliver and learn, with speed!
