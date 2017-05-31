---
layout: post
title:  "One Day. Dozens of Volunteers. Hundreds of Cans. (And a CI Tool)"
description: "How Snap CI helped a charity project track everything and the importance of understanding your users to make better technology and design choices and build better products. "
date:   2016-07-12
author: Suzie Prince
index-image-url: screenshots/tintin/tintin3-01.jpg
index-image-alt: 'tintin-dashboard'
keywords: snap-ci, deploy software, continuous deployment, faster builds, software deployment, tintin, DPA, flag day
categories: USER-STORIES
---

Kevin Yeung from ThoughtWorks' Singapore office joins The Pipeline to tell us all about how technology can help nonprofits track thousands of tin cans across a major city on a single day. He discusses the importance of understanding your users to make better technology and design choices and build better products.

### What is Project Tintin?

Every weekend in Singapore we have this tradition called “Flag Day”. On Flag Day, pre-selected nonprofits send volunteers go to train stations, shopping centers and other popular places to ask for donations by shaking a tin can. That’s why we called the project "Tintin". The one-day donation drives are called “Flag Day” because after you make a donation, the volunteers give you a little sticker to “flag you” and it basically says, "I’ve donated. Stop bothering me!"

![Tintin1](/assets/images/screenshots/tintin/tintin1copy1_Artboard-3.png){: .screenshot .big}

Our project was to help organize and manage the tin cans for the [Disabled People's Association (DPA)](http://www.dpa.org.sg/) Flag Day on April 23, 2016[^1]. The DPA wanted a better way to track their tin cans. As you can imagine, tracking these tins is not easy: we're talking about several thousand tins. After a tin is filled up with donations, it needs to be boxed and later taken back to the organization. And, we have to make sure volunteers don't run out of tins.

The process of tracking tins has traditionally been manual and paper-based. It was a paperwork nightmare. In the past, they would print all the participants and tin information out on an A3 piece of paper (23" x 33") and manually update that as the day went on. If you think about it, a few thousand tins and hundreds of volunteers… even on a piece of A3 paper, it is a lot of information to squeeze in. They wanted something better.

### What special requirements did you have for the software?

We needed a system that was easy to use in the field. It would be used for only one day, so we had one chance to get things right. The system needed to provide real-time visibility of where tins were to allow organizers to react accordingly.

UX was very important due to the variety of people using the software. DPA volunteers have different ability levels, including impaired vision and limited fine motor skills. Because of this, we wanted the software to be high-contrast and the buttons very big so that everyone could use it easily.

On top of that, it was a low-budget project. We couldn’t build new hardware, and we didn't know what devices volunteers would have. We had to decide to either build some kind of hybrid mobile app, or we had to build the same app for iOS and Android. In the latter case, we'd need to worry about Android fragmentation, and so on.

### With those specifics in mind, how did you approach this problem?

Our client said: "We need to track tins." My team's first instinct was, "Well, let's put a barcode on tins." But, when we thought about it a little more, that would mean printing a thousands of distinct barcodes to put on individual tins, then scanning barcodes on a cylindrical surface. On top of that, we didn't know what devices volunteers were going to use to scan the tins anyway.

Remembering that we were revamping that old, paper-based process, we thought long and hard about what we could do instead. Given all these requirements, we decided to track the people instead. We designed an aggregation model of tins, and boxes, and people by handing QR codes to various people. We believed it was going to work, but we wanted the users to keep us honest as well. We did some paper prototyping, some role-playing and several rounds of ideas and designs before we actually wrote any code as we were quite anxious to validate the journey, the experience, and whether the QR code was actually going to work.

![Tintin2](/assets/images/screenshots/tintin/tintin2-01.jpg){: .screenshot .big}

### What did the final software look like?

It's a web app. The entry point to the system is the personal QR code you've been assigned, which is embedded with a URL to bring you to our web app. Combined with the session information of the person doing the scanning, we get complete information about the scanner, the person being scanned, and the stage they're in so we can present the right screen with the right information. That provided us enough information to follow the end-to-end flow of tins. We used whatever QR code scanner that people have on their phone.

### Can you tell us about the technology stack?

We were also thoughtful about the technology we choose for this project. This is client is a nonprofit. Their IT system is limited to email and web browsing, maybe some Excel spreadsheets. We wanted to ensure they could maintain this project themselves after we left. So we were more conservative and went with a Rails web app. We used very minimal Javascript using a concept called progressive enhancement. We stuck to HTML5, CSS3, to make it work, and then used Javascript as necessary instead of putting in a big framework upfront.

We used to Cucumber for functional tests.

We used PostgreSQL database.

The entire software runs on AWS in Singapore. For much of the development, we ran on micro-instances, so, it's basically free.

We use event-sourcing. So, the database is an append-only data store, even though it is relational. When someone scans a QR code and says, "Hey, this tin, #123, has now gone out to Kevin," we fire a tin assigned event, and we just capture the information and stick it in the database. So, it's always append, so operation at the station is going to be very fast.

We also used Snap CI for Continuous Integration.

### Why Snap CI?

I made a beeline for Snap CI. I actually used Snap in 2013 for another ThoughtWorks project and I was keen to try it again. For this project we needed something that was fuss-free. We didn't want to wrangle with a CI tool and try to get it set up. Because Snap is hosted, has an integration with GitHub, and an easy set up made it a simple choice for us.

![Tintin3](/assets/images/screenshots/tintin/tintin3-01.jpg){: .screenshot .big}

### So you build software for use on one day? How did it go? Was it is a success? What did you learn?

Well, we could tell the volunteers and coordinators lots more information about the tins and volunteers than they could ever have got in the paper-based system.

But it was quite scary, really. What we provided, primarily, was intra-day, up to the minute, visibility into tin movements so that the organizers could respond. And, I think, we succeeded. Let me give you a few examples. There is a place in Singapore called Bishan?. It is a very populous place, very crowded, also very popular place with shoppers. Around mid-day, we actually saw they were running out of tins because we could see tins going out, coming back, getting filled, getting boxed, and we could say, "Oh, yes, they're running out of tins." The organizers were then informed, knew what happened, and could redeploy tins from elsewhere to Bishan to continue collecting money.

Another incident was, at the end of the day, we had a small handful of missing tins. Those tins didn't come back to the center. In the past, it would mean several hours of tracing the different sheets of paper, finding out what shopping center or train station the tin was dispatched to in the morning, then calling the supervisor and asking, "Hey, do you have any record of this tin? Did you hand it out to anyone?" It would be a pain.

This year, we could look at a dashboard, trace the people and tins and say, "This guy took the tin at this time, and he didn't return it." We had all the information in our system, so we could just pick up the phone and say, "Hey dude, where's the tin?" That was super awesome. The charity got all their tins back! And that was what the project was all about.

### What’s next for Project Tintin?

Their director said she wanted give this app to other organizations. We hope that in the future we can help other organizations have a better system for their Flag Days.




##### [^1]: This Flag Day was a joint effort benefiting both DPA and the Muscular Dystrophy Association Singapore.

 
Snap CI © 2017, ThoughtWorks
