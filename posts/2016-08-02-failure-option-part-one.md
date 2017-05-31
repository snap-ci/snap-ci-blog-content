---
layout: post
title:  "Failure is an Option: Part 1 of 3"
description: "Lessons I learned the hard way about scripting, database migrations, and more."
date:   2016-08-02
author: Jeff Norris
index-image-url: screenshots/failure-option/failure-option-1.png
index-image-alt: 'failure-lessons'
keywords: deploy software, continuous deployment, software deployment, continuous delivery, devops, software testing, scripting, automated testing
categories: SOFTWARE-DELIVERY AGILE
---

As software creators, we all try to prevent failures. Usually the team I work on is pretty good, but every once in awhile, something broken gets into production. How we structure the path to production can hugely influence the error's effects, and how complicated it will be to recover from it. Here are three stories from my career that demonstrate how we deal with failure and get back to business.

A few years back, I was part of a team of consultants working on a large international leasing application. This application was very complicated and was responsible for leasing millions of dollars of equipment each year. If the application wasn’t functioning, the whole business was in trouble. The leasing company's strategy for dealing with failure was to make sure that you never released a bad build. Every release considered for production was thoroughly tested by two separate regression testing teams. The performance was analyzed by two independent teams. End users exercised it for several weeks in a dedicated user acceptance testing environment.

Once we were sure that the build was ok, and all the appropriate people had signed off, about a half dozen people from my development team flew down to Tennessee to help the client put that release into production. On Friday night, the operations team would shut down the application and start the slow database backup process. We would order a few pizzas from Mellow Mushroom, the really good pizza place in the neighborhood, and pick up a few cases of Diet Coke and some extra large bags of M&M's. It was always a long night, and we knew exactly what to expect.

When the backup was done, the database administrators would start running the database migrations. It was a large dataset and there were some pretty complicated migrations, so it could take a few hours. While they did that, they would give us the all-clear to start the process of upgrading the application. Of course, since they were required to be [Sarbanes-Oxley](http://www.investopedia.com/terms/s/sarbanesoxleyact.asp) compliant, no one on the development team was allowed to touch the production servers. So we would literally stand behind the auditor and tell them “cd space install underscore directory” then “now press enter” then “cp app underscore version underscore 15” and so on for what felt like a few hours.

![Failure-option-1](/assets/images/screenshots/failure-option/failure-option-1.png){: .screenshot .big}

This was where we had our first big takeaway. *Script everything.* Our install process was eventually simplified to a single script named “install.sh”, run by an auditor that didn’t even come into the office.

After the app was installed and the database was migrated, we would check our connections with all the systems we integrated with and run a few test transactions through the system. If everything went smoothly, we would head back to the hotel for a few hours of sleep. On one particularly painful night, we had migrations that were already complicated and slow run overnight due to an index that was mysteriously absent in production. That night, I didn’t get back to the hotel until 5 a.m. and I still had to head back into the office at 6:30 a.m. for some regression testing with the end users.

This is where we had our second big takeaway. *Always dry run your database changes on real production data.* We got into a habit of doing this before each production release, even if we thought the changes would be pretty simple. Trying a representative sample was not quite enough.

On Saturday morning, when the app is up and ready with for testing with the upgraded database, we came back into the office with a few dozen end users to try out everything. It was early and we were all tired, so there was always a ton of fresh coffee, and one time, one of the project managers even set up a waffle bar. Usually the application was fine, and we only had a few minor issues that we could live with and fix with a patch in a few weeks. Occasionally, there was something dramatic that needed to be fixed immediately.

This led to our third big take away. *Get good at rolling forward quickly.* We figured out how to make quick changes to our application and get them to end users to test immediately while we finished our usual automated regression testing in the background.

We would keep at it until the users were happy, and we were ready to turn the application back on for all the customers.

### So to recap, the three lessons from this story were:

* Script everything
* Test data changes with real production data
* Get good at rolling forward quickly

I hope this story has helped illustrate why these three lessons might be relevant to your software projects as well. I'll tell you some things I learned while working on an agile project management application (Mingle) and a continuous integration and deployment tool (Snap CI) in the second and third parts of this series.

 
Snap CI © 2017, ThoughtWorks
