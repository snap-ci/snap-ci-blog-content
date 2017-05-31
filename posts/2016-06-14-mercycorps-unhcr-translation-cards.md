---
layout: post
title:  "Short on Translators, Syrian Crisis Workers Use Apps"
description: "Google, MercyCorps, ThoughtWorks, and UNHCR Join Forces to Create Aid-Focused Translation App"
date:   2016-06-14
author: Suzie Prince
index-image-url: screenshots/translation-cards/UNHCR-EmergencyLab-TranslationCards.jpg
index-image-alt: 'translation cards emergency lab'
keywords: snap-ci, deploy software, continuous delivery, software delivery, continuous integration, unhcr, mercy corps, google, translation cards, syrian civil war
categories: SOFTWARE-DELIVERY USER-STORIES
---
Since 2011, more than 4.2 million migrants and refugees have fled the Syrian Civil War in search of asylum. The migrants are all ages and speak many languages—Arabic, Pashto, Farsi, Kurdish—but they often do not speak the same language as the aid workers trying to assist them. Google, Mercy Corps, UNHCR, and ThoughtWorks partnered to create an app—Translation Cards—that helps migrants/refugees and aid workers overcome language barriers in these chaotic environments. All organizations involved say they could not have done this project alone. Natasha Jimenez, Patrick Dale and Pavlina Lejskova from ThoughtWorks joined The Pipeline to tell us more about the app and how Snap CI was a part of their solution.

![UNHRC Emergency Labs translation cards](/assets/images/screenshots/translation-cards/UNHCR-EmergencyLab-TranslationCards.jpg){: .screenshot}

#### Can you tell us more about Translation Cards?

Sure. [Translation Cards](https://github.com/translation-cards/translation-cards) is an Android application to help refugee aid workers communicate with refugees in environments that are really hectic such as those in FYR Macedonia where thousands of migrants are arriving daily. In these environments, aid workers lack connectivity to any sort of data or internet, have access to very few resources, and are understaffed. Having done [research in the field](https://docs.google.com/presentation/d/1lGP95eSvFrQCUTDu2Q4zG1K9vSB6X8lY7cxzyFy9CbU/edit#slide=id.p), our data showed that aid workers and refugees wanted something really simple that they could use to communicate with each other.


![translation cards](/assets/images/screenshots/translation-cards/translations.gif){: .screenshot .fixed-size}


#### What technologies and tools did you use to build it?

We are using:
* Android SDK 23
* SQLite 4
* Java 7
* Frameworks: Robolectric, Butterknife, Espresso, JUnit + Mockito
* Gradle
* Snap CI
* Android Studio
* AWS S3 Bucket

#### It’s great to see you using Snap CI. How important was continuous integration and delivery to this project?

It was important to get regular updates of the app out there.

I think one thing that Snap CI did great and helped us achieve—and I guess you can call it a form of continuous delivery—was being able to very easily get an Android application package to the partners at UNHCR and Mercy Corps. They were really able to easily access it: they could just click a button, and go there.

If you’re an aid worker from [UNHCR](http://www.unhcr.org/en-us/syria-emergency.html) or [Mercy Corps](https://www.mercycorps.org/tags/syria-crisis), it's nice that you can just go to that URL and download it on your phone and quickly say, "This looks good, I'm cool with my content being on here or not." That part of delivering it to the people who have their names attached to the content inside of the application, that was super important to be really easy, and it was!

We’d do that every week, or every other week. We'd send them that link and get them to check it over before we published to the [Google Play store](https://play.google.com/store/apps/details?id=org.mercycorps.translationcards).

#### How do you use Snap CI on this project? Was the staged nature of the deployment pipeline important to you, or that's kind of how it worked out?

It was important. The [stage set-up](https://github.com/translation-cards/translation-cards/blob/master/docs/build-pipeline.md) was intentional.

We put in the first stage to ensure that the app would compile. Part of that stage was checking that the tests still syntactically make sense. It was nice to initially have that stage there and be able to say to each other, "Hey, our tests don't really compile, can we try to fix that?" It got us all more motivated and involved with the testing part of the application.

The unit test stage was pretty intentional. We followed that from your documentation. Then the last stage, the upload S3-which I think is slightly different-was like our version of a QA environment, or like a staging environment.

#### Is Translation Cards being used?

Yeah, there's people from Mercy Corps and UNCHR testing it with some of their field workers. They were testing it in Greece, and now they are testing it with some more languages.

#### What have you learned from the feedback?

The feedback was powerful, it gave us a lot more motivation to keep on working on it. One of the refugees said it was one of the first times they felt actually connected with a field worker, and could carry on a conversation. That right there was our first real "field release", and from there we were like “this can definitely help people”.

We were expecting a lot more constructive feedback: more things we could improve, or things that were frustrations. But we got a lot of, "This tool is really useful."

It's helpful to actually be able to communicate in real-time. Someone even said it was helping to increase morale in the field. Their conditions are really desperate right now, it's a really tough environment to be in. To be able to actually create connections between people, I think that is really powerful. It was really cool to hear that from their side.

#### What’s next for the project?

We're hoping to have a couple of other components with the app.

The first is a deck store. Right now we have this Android app. When you download it from the app store, you need to also download the translation card decks for whatever specific type of work you do. Maybe the medics will have a deck for what they're doing with it, the languages they need, and the translations they need. Other roles in other countries will have other decks.

We have this idea of where we'd love to have a type of app store, but for decks. It will be a deck store, and people can just access it and look at all the different decks available, created by all the different organizations. Our plan is that from the app, you'll be able to access the store, you'll be able to click on the deck you want and it will download to the application.

The second is a better workflow for creating the decks. We want it to be a very smooth process. Right now it's a bit complex for the organizations to be able to put these decks together, and then email it out. We just want to make it all smooth as possible, and we think Snap CI will help us with that. Snap CI will be integral to that workflow and ease of use.

Currently we have organizations put in their decks in GitHub and we build a deck from their content. There are a lot of manual steps and lots of errors. It is hard for people to find where the errors are. This is where a good automated deployment pipeline can be really important. We will have a Snap CI build pipeline that will bundle together all the files into the correct deck structure, then it will release that to a hosting environment, where they can just get immediate feedback on it.

In future iterations we will add stages to check to see if the deck has an invalid syntactic form, so that someone can easily see where the errors that they put into the file actually are before testing it. We're going to use Snap for that part and it'll be pretty sweet.

* * *

You can check out translation tools at the [Google Play store](https://play.google.com/store/apps/details?id=org.mercycorps.translationcards) and follow them at the [World Humanitarian Summit](https://storify.com/storybyoliver/translation-cards-coverage-on-twitter), and the ICT4D Summit in Nairobi.

Photo credit: Translation Cards being used in the field (credit: [warnes@unhcr.org](mailto:warnes@unhcr.org), UNHCR)

 
Snap CI © 2017, ThoughtWorks
