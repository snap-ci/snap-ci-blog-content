---
layout: post
title:  "Zero, One, Infinity"
description:  Willem Louis van der Poel came up with the zero, one, infinity heuristic. We find that remains a useful way to think of design parameters and constraints.
date:   2016-09-27
author: Badri Janakiraman
index-image-url: screenshots/zero-one-infinity/zero-one-infinity-zoi.jpg
index-image-alt: 'zero one infinity'
keywords: continuous deployment, continuous integration, caching, design, heuristics, rules-of-thumb
categories: SOFTWARE-DESIGN DESIGN-HEURISTICS
---

[Zero, one, infinity](https://en.wikipedia.org/wiki/Zero_one_infinity_rule) is a [fairly well documented](http://c2.com/cgi/wiki?ZeroOneInfinityRule) design rule-of-thumb that nonetheless seems less well-known than some of its counterparts.

## "Allow none of foo, one of foo, or any number of foo" - Willem Louis van der Poel (paraphrased)

![zero one infinity](/assets/images/zero-one-infinity/zero-one.png){: .screenshot .small}

The rule suggests that when considering a design element, arbitrary numbers greater than 1 should be eschewed, if at all possible. That is, if more than one of something can exist, there should be no logical reason that there can't be infinitely many of them. For example, consider a web application that is fronted by a load-balancer. The application may be designed to work only when running on a single server. Limitations such as storage of session state, local disk-based operations, etc., might prevent this application from running correctly on more than one machine. However, if those issues are resolved and the application is made truly stateless with all durable state extracted out of the application itself, then the application can be scaled by launching as many servers as needed. I.e., once the limit of having a single copy of the application server is removed, there can be an arbitrary number of servers running the application to which incoming requests can be directed.

Of course, the real world has real constraints. Machines have fixed word lengths, algorithms come with complexity metrics, network interfaces and data buses have bandwidth limits and these practical considerations will constrain how closely reality matches the design ideal. The rule doesn't make these limits disappear in practice. What it does provide, though, is a way to detect when something is fishy. If a design is dependent on a key arbitrary number, it should make you pause and question what the origin of that number is - and perhaps even find the reason documented in something other than tribal memory.

For a couple of slightly less pedagogical examples, here are a couple of ways in which we have used this on Snap in the past  and how they worked out.

###### Build host count

When we initially launched Snap, we had exactly one build host. All our workers ran on the single host. The worker names, IP addresses, and all other details were configured without ever taking into account the fact that the workers could live and process builds on multiple hosts. This worked well enough until we needed to scale to the second host. At that point, we could have chosen to have done just enough work to make sure that we had a second build host. However, we decided to bite the bullet and make it possible to have an arbitrary number of build hosts on the backend. This worked perfectly well for the first few hosts we brought up.

Fast forward a few months to a deployment that was launching new build hosts. During this deployment, as we were creating the workers, we had a few failures that manifested themselves that when the workers started processing builds. Turns out, the algorithm we had used to assign IP addresses to the workers was flawed. It would, in certain cases, assign invalid IP addresses - such as 10.0.100.415. The design only blew up when the last octet filled past the 255 mark which is the highest valid value to have.

So how could thinking about 'zero, one, infinity' have helped in this case? Turns out there were a couple of better options that would have prevented us from hitting the magic 255 number.

The first, if we wanted to keep static IP allocation, would be to just mod the last octet's value by 255 to prevent overflow. Turns out that in our network that would have been a perfectly valid thing to have done. If we wanted to move away from static IP allocation, we could have set up a DHCP server that allocated the IP addresses. Now - to be fair, this too would have a limit. We might have run into issues when we allocated the complete range for the third and fourth octets, i.e. approximately 65,000 addresses. However, firstly, that limit is a lot larger than we are likely to encounter soon (famous last words). Secondly, this addresses the caveat I mentioned - which is that the real world has limits. It would be good to find solutions that don't rely on arbitrary numbers as far as possible, but when practical considerations force your hand, it would be good to have the design constraints and limits documented.

###### Caching

The second example happened more recently when we were discussing the implementation of the durable cache that we are in the process of building in Snap. The question came up as to what parts of a Snap worker should be cached to speed up builds. The obvious responses we had were:

* Nothing - no caching. The workers would start from scratch every time. This would result in the most reliable builds since there would be no accumulated cruft between builds that lead to unreliable build results. However, this would also lead to the longest build times.
* The entire worker - cache everything. This way, the entire machine state would be persisted and restored for each build. This was attractive because it gave the best impression of Snap behaving like the "build machine under your desk" or "the developer workstation" which rarely changed state. On the flip side, the build results would be highly unreliable and prevent the Snap team from ever upgrading packages or maintaining your build systems.
* Cache the $HOME directory - cache just one well known directory. This provided the best midway point between the two prior options. A lot of systems such as Maven and SBT already use the $HOME directory and by picking that location, we met a lot of the expectations that people had about what state would be maintained across builds.

In the above example, the three options correspond to the zero (no cache), infinity (everything cached) and one (just the $HOME) conditions respectively. As you can see, sometimes it turns out that picking "one and only one" is a perfectly valid option.

If you have had examples where you have deployed this thinking, do share it in the comments. We would love to hear from you.

 
Snap CI © 2017, ThoughtWorks
