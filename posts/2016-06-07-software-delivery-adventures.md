---
layout: post
title:  "Breeding Cobras, Brown M&Ms, and other Software Development Adventures"
description: "If you are going to incentivize certain behavior in your development team, make sure that you are checking regularly to make sure that you are not encouraging them to breed cobras"
date:   2016-06-07
author: Jeff Norris
index-image-url: screenshots/software-development-adventures/software-delivery-actions.jpg
index-image-alt: 'Software Delivery Adventures'
keywords: snap-ci, software development, continuous delivery, continuous integration, freakonomics, software delivery, software teas
categories: SOFTWARE-DELIVERY
---


Like a lot of you, I really enjoy the Freakonomics books and podcasts. Stephen Dubner and Steven Levitt do an amazing job using economics to understand diverse topics ranging from teachers cheating in schools to the relationship between abortion legality and crime rates. In their book, "Think Like a Freak: The Authors of Freakonomics Offer to Retrain Your Brain”, they talk about some interesting topics including incentives and how they affect our behavior. This information can be used to improve your team’s effectiveness building software. Here are a few of the practices that you can use.

#### Stop encouraging people to steal

At the [Petrified Forest National Park](https://www.nps.gov/pefo/index.htm) in Utah, the rangers have a problem with visitors stealing petrified wood. In hopes of reducing theft, park officials tried several approaches and tested out several anti-theft signs. They found that signs stating how much petrified wood had been stolen, or that too many people did it, actually increased thefts because it set up stealing as something "normal" or that a lot of people did. The only sign that decreased petrified wood stolen from the park was one simply requesting visitors not steal.

Having a sign stating "Visitors have stolen 2.2 tons of petrified wood each year" at the park is the equivalent of having a sonar report that shows your technical debt slowly increasing, or a code coverage report that shows that you hover at about the same coverage for months after month. If you are trying to use metrics like these to help shape your development practices, it is more important to show how values are trending rather than their absolute values. On our projects, we usually do this by picking metrics that highlight problems on the project and break the build when these get worse. As we continue to make progress on these metrics, we incrementally ratchet down this metric until it is no longer an issue for us. As we make progress on a particular metric, it usually stops being the most important issue on the project and we pick another metric to focus on.

#### Watch the actions, not the words

![software delivery takes action](/assets/images/screenshots/software-development-adventures/software-delivery-actions.jpg){: .screenshot .big}

When people were surveyed about conserving energy, the primary reason that people gave was related to caring for the environment. It is great that they value the environment, but when their behavior was tested based on their actual energy consumption, they actually responded better to social pressure, seeing that their neighbors were conserving energy and that if they want to be a good member of the community, they need to conserve energy too. In software development, if you ask people if they care about quality and time to market, they all say yes. The words aren’t enough: you need to look at the underlying reality and measure things like the cycle time it takes from deciding that you want to work on a feature until that feature is in the hands of end users. If it is taking a long time to get from decision to production, it could be people do not care as much as they say they do, or for different reasons.

#### Don’t breed cobras

In colonial India, a Delhi official decided the city had too many cobras. He created a bounty program: residents would be paid for any dead cobras they turned into the government. Unfortunately, some resourceful Delhi residents responded by starting cobra farms. The government soon caught onto the loophole and discontinued the bounties. Residents released the farmed cobras, leading Delhi to have many more cobras than it did before the bounty program. The lesson learned: any time you incentivize something, people will game the system. So it is important to assess what the impacts of your incentives actually are.

In software, I have seen incentives go badly with code coverage of testing. For example, to deploy a project to SalesForce, you have to have 75% code coverage. On one project I inherited from another team, they practiced “assertion-free” testing, where you call all the methods, but don’t bother checking the results. This let the code deploy to production without bothering to check if the application actually functioned. We essentially had to delete the tests and start over again. If you are going to incentivize certain behavior in your development team, make sure that you are checking regularly to make sure that you are not encouraging them to "breed cobras".

#### Check for brown M&Ms

![architectural assessment](/assets/images/screenshots/software-development-adventures/software-architectural-assessment.jpg){: .screenshot .big}

The band Van Halen had a [53-page document](http://www.thesmokinggun.com/documents/crime/van-halens-legendary-mms-rider) listing all the requirements for a gig venue. In this document, they specified things like electrical and safety requirements, but also demanded that the dressing rooms have a bowl of M&M’s with "ABSOLUTELY NO BROWN ONES". When the band arrived at the venue, they could quickly check how conscientious it was by looking for brown M&M’s in the bowl. If the venue's managers missed the brown M&M’s requirement, then they probably didn’t pay enough attention to the other details, and the crew would need to double-check everything else to make sure they didn’t skip things like rigging that would affect the safety of the band. When I am joining a team or doing an architectural assessment, there are a few questions that I ask to see if they have missed the "brown M&M’s".

* Where are database changes stored and how they are related to underlying code changes?
* How often does each team member commit to the master branch?
* How long did the last release take from starting development to being in production?
* Are there any secrets like passwords or access keys stored in version control?

If I spot any "brown M&M’s", I know that I will need to be careful and check a wide variety of related quality practices.

As the these stories show, incentives and game theory can have a huge impact on the way that your team operates. These impacts can be both negative and positive, and you need to be careful that you don’t accidentally make things worse. I would encourage you to "Think Like a Freak" and apply these practices on your development team.   
