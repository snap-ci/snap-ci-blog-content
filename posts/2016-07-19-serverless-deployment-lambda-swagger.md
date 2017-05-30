---
layout: interview
title:  "Going Serverless with Snap CI, Amazon API Gateway, Lambda, and Swagger"
description: "How serverless architecture allowed a news site to try new things and move faster."
date:   2016-07-19
author: Suzie Prince
index-image-url: screenshots/the-tab/thetabteam.jpg
index-image-alt: 'the-tab-team'
keywords: serverless, deploy software, continuous deployment, faster builds, software deployment, the Tab
categories: SERVERLESS-ARCHITECTURE USER-STORIES
---

We talk to Snap CI user Richard Coombes from [The Tab](http://thetab.com/) about their use of serverless architecture. As we [have heard](https://blog.snap-ci.com/blog/2016/05/03/serverless-architecture-burst-traffic-applauze/) from other Snap users at Applauze, serverless technologies support fast moving organisations by facilitating their ability to scale quickly. The lower costs of trying out new ideas is a big reason why going serverless makes sense for quick-moving organizations like The Tab who need to deploy quickly and easily. Hear from Richard as he tells us more.


#### What can you tell us about The Tab and what you are working on?

The Tab is a platform for young journalists to report and write, professionally and independently. It was founded at Cambridge University in 2009.

Our core site is in WordPress. That’s our business critical thing. But then, the real magic that we do is around the outside of that. As an example, we have a Facebook [messenger bot called Tabitha](https://www.messenger.com/t/TheTabOfficial) that keeps authors updated with how their stories are performing, and tells readers when stories are breaking. As another example, we have an onboarding site that gets new authors to sign up. We've also got a native app which gets a lot of push notifications.

So these parts around the main site are what the tech team here is really working on and we're trying out unknown things quite a lot of the time. Specifically in the online publishing space. It's a fairly new place to be. Some of the ideas that we come up with are great, and others don't work out so well, so we build a lot of data and analytics and we see how things do. I'd say with a decent chunk of stuff we develop we then pivot the functionality once we learn how our authors use it. Or we learn something from it and turn it into something else.

![Tab2](/assets/images/screenshots/the-tab/thetabteam.png){: .screenshot .big}

#### Can you tell us what particularly got you interested in serverless architecture? What are you trying out right now?

It's a bunch of reasons. Firstly, we're effectively a startup and we don't really have an infrastructure engineer, or anyone looking after servers. So whatever we build, one of the devs is going to have to monitor and keep up to date. If a server goes down, one of us is going to get a call and it might be on the weekend or outside of work hours.

Secondly, another big thing for us is there's no real overhead, so we can try things out pretty quickly. We're not fine-tuning installs of operating systems or other things. It means we can build things quickly. As a startup, building things means that they might not be successful, we might throw it away and move on to something else. We're not really trying to eke out the last bit of power out of a piece of infrastructure.

The last thing is that there's no commitment in terms of ongoing costs and it removes that whole need to really plan what servers you're going to have, and then the difficult decisions about who gets access to what, etc. With serverless through the cloud, it's such a low cost that we can try things out. If they don't work, it's fine. If they do work, potentially we can go back at a later date, and change the settings. But there's no kind of upfront planning needed, it's kind of try things and get on with building them and leave the other stuff to someone else who's going to do it better than anyone here at our set-up, because we're not network engineers or anything like that.

#### Tell us a little bit more about the architecture you have.

We have a WordPress core platform and then we've built what's effectively a data warehouse. We have a bunch of micro-services, some of which are in Lambda, and some of which are Beanstalk workers. They suck in data from various places-so they ingest stuff like Facebook Insights data, and some of the core WordPress content such as the stories and the users.

The concept is that the API is our main place you go to to get data that's not in the core platform. We call it ["Skynet"](https://en.wikipedia.org/wiki/Skynet_(Terminator)) because we're nerds, but it's where we produce what we call "meaningful insights". So we've wrapped that database in an API (using API Gateway), and various services connect to that API. For example, our native iOS app uses the feed from there. We apply some trending data to it so that whenever someone comes to the native app, they always get something new to look at it. It's going on Google realtime viewing data.

It's also used for the actual main Tab site. If you log in as an author, when you look at your stories on the main site, you'll see the number of people looking at them right now, page views and how you rank against other members of your university or college.

#### What tools are you using? And how well do they work for you?

We're using Amazon's API Gateway and Lambda quite extensively. The database we're using with the API is an RDS MySQL instance.

It's definitely a mixed picture with API Gateway. My impression is that API Gateway does very much feel like a V1, perhaps a V0.5 product. The UI on the Amazon side is really horrible, like to the point where none of the API methods are even sorted alphabetically. It's probably the worst of the AWS user interfaces to work with, so that's why we're so keen to try and automate as much of it as possible.

I don't think the concept of stages for deployment works very well in API Gateway because you can develop something, select where you want to deploy it to, but you can never see the multiple versions that exist. It just kind of feels tacked on and, again, it's all done through the UI.

![AmazonUI](/assets/images/screenshots/the-tab/amazon-ui-with-caption-01.png){: .screenshot .big}

It's been a bit of a learning curve to get [Swagger](http://swagger.io/) working and then use Snap CI to push... We actually just have separate API Gateway instances. But using Snap CI and using Swagger, we've kind of massaged it enough that it works pretty well.

I'd say the only issue we really face is that when there are errors in deployment, the Amazon-side logs are not massively verbose. But as a development process, we knock out API methods really quickly now and they're super-reliable and they scale. Because we're publishing stories, there are times when traffic jumps up quite a lot. In those situations, it's a huge advantage to us that the API Gateway has all the caching built in and we never see any of the server stuff. I think that itself makes the growing pains-dealing with Amazon's slightly clunky documentation-worth it.

#### How do you keep your code clean when using this many services?

We were using a large number of Lambda functions but then we realized that packaging and deploying them-especially when we had dev and test versions, and multiple environments-resulted in exponential complexity. So we combined them and brought them all into one codebase: so there is only one named Lambda function we're pushing to. We have an index.js that then has multiple routes depending on an input parameter to package all the functions needed by the API.

For notifications, we've piped all our Lambda notifications into CloudWatch, and now we use that to alert us in PagerDuty if anything critical happens. Otherwise, it would be hard for us to keep track of what's actually coming through just because the sheer verbosity of Lambda.

We still had some challenges trying to get that codebase properly unit tested. We found a node library called aws-lambda-mock-context, a [mock context library](https://github.com/SamVerschueren/aws-lambda-mock-context), in which the code can get a bit verbose, but it works, so we can actually test those Lambda functions as if they were running on Lambda. It was initially quite hard to develop and test without coding things manually, but we've kind of squared that off as well.

#### What is your testing story?  What is your local development story?

You can test a lot of the Lambda stuff using the mock context library and Mocha, these get triggered on build and we try to take a test-driven approach to development. A lot of the interesting work happens in the functions, so most of the logic and things like that can actually be tested over there. In addition, we deploy frequently so the deltas that get deployed are not significant each time. Due to that, even if something does go wrong, we'll know exactly where it did go wrong because there's not too much that changes between different versions. We have some Runscope tests that run when a deployment happens so we know if it deployed successfully.

#### In a [previous post](https://medium.com/@coomberc/building-a-testable-continuously-deployed-api-using-amazon-api-gateway-3654addd5a0e#.9n1cav9jn) about this process, you said “But you can quite quickly get stuck in an untestable and hard-to-deploy mess.” Why?

If you didn't use any tooling, you didn't use anything like Snap CI or any of the unit tests, it would be pretty easy to get stuck. If you were making changes to the API Gateway just using the clunky UI and... It's probably true of all development but it felt particularly bad here because you have code in places that you don't even know it's there. I think getting a deployment process in place for stuff like API Gateway and Lambda is probably even more critical than most places where you can zip up a project-and with lots of small serverless microservices you can’t just hop on a server and see what’s running.

##### Special thanks to Richard Coombes from The Tab for speaking with us. You can read his related piece, ["Building a testable/continuously deployed API using Amazon API Gateway,"](https://medium.com/@coomberc/building-a-testable-continuously-deployed-api-using-amazon-api-gateway-3654addd5a0e#.9n1cav9jn) on Medium.
