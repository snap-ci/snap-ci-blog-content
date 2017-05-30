---
layout: post
title:  "Why Continuous Delivery and DevOps are Product Managers’ Best Friends"
description: "Why should you care about these practices? Here’s why continuous delivery is a product manager’s new BFF."
date:   2016-09-06
author: Suzie Prince
index-image-url: screenshots/pm-guide-cd-devops/snap-ci-after-devops.png
index-image-alt: 'devops and cd for product managers'
keywords: snap-ci, deploy software, continuous delivery, software delivery, devops, software deployment
categories: CONTINUOUS-DELIVERY DEVOPS
---


In the [first part of this series](https://blog.snap-ci.com/blog/2016/06/28/continuous-delivery-product-managers/), I explained what these buzzword-y “Continuous Delivery” (CD) and “DevOps” things are and started to touch on why you should care. But really, why should you care about these practices? Here’s why continuous delivery is a product manager’s new BFF.

### Continuous Delivery is Transformative to Businesses

The main reason you should care about CD and DevOps is that it is transformative to businesses. Time and time again, organizations you admire, which produce products people love, reveal that they are doing Continuous Delivery and/or Continuous Deployment, and have a DevOps culture. Some good examples of companies that do this would be [Etsy](https://www.infoq.com/news/2014/03/etsy-deploy-50-times-a-day), [Netflix](http://techblog.netflix.com/2015/11/global-continuous-delivery-with.html) and [Amazon](http://joshuaseiden.com/blog/2013/12/amazon-deploys-to-production-every-11-6-seconds/).

But CD and DevOps have not just helped Silicon Valley darlings to get big: CD has helped transform organizations all over the world. The Guardian is a [great example](https://www.theguardian.com/info/developer-blog/2015/jan/05/delivering-continuous-delivery-continuously) of an older business who embraced the benefits CD brings by moving from a bimonthly release cycle to making tens of thousands of automated deployments a year. And the benefit for them?

>“What has been transformative for us is the massive reduction in the amount of time to get feedback from real users.”

Likewise, [teams at Expedia](https://www.thoughtworks.com/clients/expedia) embraced CD, and within the first three months of using the new practices, they were able to run more experiments on their site than in the entire previous year. This ability fueled increased conversion rates, and led to significantly improved business performance.

And these examples are not outliers, my friends, oh no. In the [2014 State of DevOps](https://puppet.com/resources/white-paper/2014-state-of-devops-report/thank-you) report, Puppet Labs asserts with confidence that:

>“IT performance positively affects overall organizational performance”.

This means that if you can up your IT and delivery game, you can up the game of your whole organization.

So there you have it. That’s the big, meaty, “you should totally do it!” justification screaming at you. Share it with your manager and your manager’s manager.

### Why Does Continuous Delivery Matter to Product People?

But how will the product manager, the marketing lead and the support gurus out there see the benefit of these practices in their day-to-day work lives? Let’s find out.

#### It Will Help you Get Feedback Sooner

The main reason I love to deliver continuously is to get product feedback and validation sooner. The moment a new product or feature gets delivered to your customers, you start to get real feedback. Yes, there are a bunch of things you can do to test ideas before this with interviews, mockups, prototypes and user tests. [I love these practices too](http://www.mindtheproduct.com/2015/05/find-right-customers-product-development-interviews/), and I am not advocating “build it and they will come”. Yet there is nothing like the rubber-hits-the-road feedback you get, having pushed a new feature to production, to really learn if it achieves what you hoped it would – or not.

#### It Increases Responsiveness

When your team can deliver each and every build to production, you get to choose if each and every build goes to production. That means the time-to-market for the feature your customer asked for last week is, theoretically, just the time it takes to build and test it. If an idea is your top priority, why would you want to wait before delivering it? With CD, you don’t have to. No longer do you need to say “It’s built …  and will be released in December”, unless you actually want to. Instead you get to say: “The team pushed that to production while we were meeting. What do you think?”

This responsiveness works in a million ways to help you, the product manager. When the next [Heartbleed](http://heartbleed.com/) bug comes out, you can get a fix out before lunch. When your startup’s biggest customer needs a change, you can get it done and deployed overnight. Or let’s say your once-great-but-slowly-fading industry giant is being killed by small, lightweight, agile competitors, and you’re in charge of delivering value to customers this quarter: Let CD be your friend. It will help you solve problems quickly, fix issues as they occur and stay on top of customer needs.

#### It Reduces Waste and Risk, and Saves Money

I would need numerous hands to count the number of times I have seen product managers and clients add scope to a release, horde bugs, or refuse to deliver an MVP because they thought the next release was the only chance they had to complete a feature. Just last week a friend of mine at a well-known healthcare company said:

>“We missed the May release train, so we go live in December. And that means January because of holidays, so we’re just doing some extra stuff until then.”

Seriously, they are just “doing some stuff” for 10 months while they wait for the [release train](http://www.scaledagileframework.com/agile-release-train/) to pull into the station. And I bet they’re not the only ones.

If you’re not following me, here’s another example of what happens when you have to wait to release.

Imagine you have an app store and you want to test out whether a ratings and review system helps users find the best apps on your store. If you are delivering continuously, you could start with a simple “thumbs up” feature – User gets apps they like and so give a “thumbs up”. Does this solve your problem? If yes, sweet – you are done. Next problem please. If no, then let’s see if a five-star system is better. Or reviews. Or scores out of ten.

#### Continuously Deliver Small Chunks of Value

If you can deliver value incrementally and iteratively, you can deliver less at each stage, get to the right solution faster and get better ROI on your investments. If you weren’t delivering those features regularly, I bet you that you would design, develop and deliver a whole five-point rating systems with user reviews. Then when you were thinking about reviews, you’d realize that you need to run them through a moderator, or remove blacklisted words so the reviews don’t have profanity in them. Suddenly you’re building the world’s best text moderation tool, and all you wanted to do was show users which apps were great and which were terrible.

In this way (and by avoiding traps like those I outlined above), CD reduces waste. It saves your business from building stuff it doesn’t actually need, allowing you to focus on value. It helps you meet customer needs sooner. It helps you to quickly understand how your product strategy is working. It helps you get ahead of your competitor more quickly. It will help you with your “business hat” on, not just your “technical hat”.

#### It Facilitates the Creation of Higher Quality Products

>“Quality is everyone’s responsibility. One of the significant benefits of CD is that it gives better visibility to everyone associated with a software project of the state of that project and so anyone can react to something that doesn’t look quite right.”
>– Dave Farley, co-author of Continuous Delivery

Although this might not be something you think about all the time, service quality is as important a “feature” as the actual code or tangible products you deliver. If your releases never result in a downtime, if your software is always available, if users never find a bug, then your customers’ opinions of your software will be higher, your referral rates will be higher, and you’ll have won your market.

#### It Leads to Releases That Are Sane and Calm

CD really does make releasing boring. My previous team complained to me that we hadn’t had a release celebration lunch for months. And it wasn’t because we weren’t releasing – we released everyday! It was just that we hadn’t had that adrenaline-fueled 24-hour period of “We will we get the feature out on time!” intensity followed by an evening unwinding in the pub that is often associated with a software release.

When you get into the habit of releasing, it becomes a simple part of how your team works. At 4 p.m. every day, our teams asks each other “what is ready to deliver?”. The product team, marketing lead and support team discuss the implications of what’s ready, and they decide if they want to release or not. It’s that simple. (You’ll remember from [the first post](http://www.mindtheproduct.com/2016/02/what-the-hell-are-ci-cd-and-devops-a-cheatsheet-for-the-rest-of-us/) that this choice is one of the key differences between continuous delivery and continuous deployment).

If the product manager wants to build a little more feature, they do that. If they want to test it with a few users first, they do that. If the marketer chooses to wait for PR reasons, they can do that instead. And if the support team says they need a particular fix for one high-value customer, it goes out. Calmly.

#### It Makes For Better Teams

As discussed in the [first post](https://blog.snap-ci.com/blog/2016/06/28/continuous-delivery-product-managers/), DevOps is about culture. In order to get these practices right, people need to collaborate, meaning work together, break down silos, share goals and act as a team! As you can imagine, there are many side effects of the push to DevOps that make your team function better. As shown in the scenario above, because you are talking about deploying every day, you are likely to be talking about quality every day, as well as delivering value to customers.

The more the team centers around these fundamental qualities of your software, the better the software is likely to be. Likewise, when team members are not siloed, they understand each other, and each other’s work, better. The less they are feeling the individual burden of a deployment or bug-fixing or a release test cycles, the more time they have to participate in team activities, share feedback, and help the whole team reflect and grow. It’s warm and fuzzy, and maybe even sounds idealistic, but it’s true in my experience.

#### Too Good to Be True

Alright, I’ll stop here. Honestly, there’s actually more reasons that I can think of as to why I want you to embrace CD practices, but I don’t want to sound over-zealous! And I have to say: it won’t be easy. Transitioning to a continuous delivery and DevOps culture is not all sunshine, unicorns and rainbows. It’s hard work on the part of your engineering and operations staff. And you. You product people have got to work hard to get this change done. When you move to this way of working, expectations change, your way of working changes, you have to change. But when you do, the payoff is immense.

#### Stuff You Should Probably Read

* [https://www.theguardian.com/info/developer-blog/2015/jan/05/delivering-continuous-delivery-continuously](https://www.theguardian.com/info/developer-blog/2015/jan/05/delivering-continuous-delivery-continuously)
* [https://leanpub.com/buildqualityin](https://leanpub.com/buildqualityin)
* [http://papers.ssrn.com/sol3/papers.cfm?abstract_id=2681909](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=2681909)
* [http://www.cio.co.uk/insight/r-and-d/three-views-on-continuous-delivery-3601806/](http://www.cio.co.uk/insight/r-and-d/three-views-on-continuous-delivery-3601806/)
