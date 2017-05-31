---
layout: post
title:  "Continuous Deployment of a Static Website to AWS with Snap CI"
description: "Here's how you use Continuous Deployment to delivery a static website using AWS and Snap CI"
date:   2016-2-09
author: Danilo Sato
index-image-url: screenshots/aws-static-website-deploy/aws_site_estatico.png
index-image-alt: 'developing for mobile'
keywords: AWS, continuous deployment, continuous delivery, GitHub, deploy website
categories: AWS continuous-delivery
---


## Implementing a deployment pipeline does not have to be difficult

Implementing a [deployment pipeline](http://martinfowler.com/bliki/DeploymentPipeline.html) does not have to be difficult, and this article will show you how to use ThoughtWorks' Snap CI to deploy a static website to the cloud using Amazon Web Services (AWS).

In the book ["Devops na Prática"](https://casadocodigo.refersion.com/l/b84.1397) (currently only available in Portuguese), I show how to configure a deployment pipeline for a non-trivial Java web application. One of the main objectives of the book is to show how to apply the principles of DevOps and Continuous Delivery in practice. However, we had to make choices about which tools and libraries to use in our examples. This article is an extension to the book's content to show that we can apply the same practices using other tools, even for a completely different type of application.

As part of the book launch, we built a static website to aggregate information, testimonials and act as a marketing landing page for the book.

![DevOps in Practice](/assets/images/screenshots/aws-static-website-deploy/devops-in-practice.png){: .screenshot .big}

The website source code is available on [GitHub](https://github.com/dtsato/devops-book-site) and the production environment is running on Amazon, using these AWS services: [S3](https://aws.amazon.com/s3), [CloudFront](https://aws.amazon.com/cloudfront), and [Route 53](https://aws.amazon.com/route53).

## Continuous Delivery or Continuous Deployment?

This is a question I hear frequently. In this [post](http://continuousdelivery.com/2010/08/continuous-delivery-vs-continuous-deployment/), Jez Humble explains the difference. Continuous Deployment is the practice of releasing every good commit that goes through the pipeline to production, without any manual intervention. Continuous Delivery is the practice where every good commit can potentially go to production, but the moment when the release should happen is a business decision.

To better understand the difference, consider two companies with different business models: the first one sells software as a service (SaaS) while the second one distributes software to their customers and charges license fees depending on how many copies are installed.

While the first company can decide to do continuous deployment, releasing to production the most recent changes, the second company might prefer a slower delivery cadence because each new release requires repackaging and distribution of the software upgrade across their customer base.

Regardless of which approach you decide to use, they both require a change in mindset: any commit can be chosen to go to production. This requires discipline, automation and this is where DevOps practices can help. In particular, you have to create a deployment pipeline that models your current software release process in an automated fashion. Every commit that goes through the deployment pipeline successfully is a release candidate that can go into production.

## Snap CI helps you go from Continuous Integration to Continuous Delivery

Snap CI integrates with GitHub and supports applications written in various platforms – Ruby, JRuby, Python,  Java, JavaScript, Rails, Node.JS, PHP, Groovy/Gradle, Scala, Clojure, C/C+ or Android – using different types of databases – Sqlite3, PostgreSQL, MySQL, Redis, CouchDB, and MongoDB. It also supports different deployment options if your production environment is on Heroku or AWS. You can read the [documentation](https://docs.snap-ci.com/) to learn more details about the platforms and deployment options supported by Snap CI.

## The production environment on AWS is relatively simple

We are using S3 (Simple Storage Service) to store all our static files: HTML, CSS, Javascript, images, etc. S3 acts as a kind of file system that is highly resilient, where each object is replicated multiple times to increase the durability in case of a disaster.

There are two basic concepts on S3: buckets and objects. A bucket acts as a namespace that is uniquely identifiable across all of AWS. Inside the bucket you create objects that can be files or folders. Both buckets and objects have security properties that allow you to define their access rules.

Although S3 can be used independently to host an entire static website, our production environment also uses CloudFront that acts as a Content Distribution Network, or CDN. A CDN is a network of edge nodes spread around the globe that act as a local cache for the original content, which in this case is hosted in S3. When users try to access a given URL, they are routed to a node that is geographically close to their location, offering a better performance to fetch the content.

Finally, we also use Route 53 to configure the DNS host zone for our static website. With the integration between AWS services, we can use Route 53 to redirect our website's entry point URLs ([www.devopsnapratica.com.br](http://www.devopsnapratica.com.br/) and [devopsnapratica.com.br](http://devopsnapratica.com.br/)) to our CDN distribution in CloudFront.

![AWS Static Site](/assets/images/screenshots/aws-static-website-deploy/aws_site_estatico.png){: .screenshot .big}

Amazon has a well-documented [tutorial](https://docs.aws.amazon.com/gettingstarted/latest/swh/website-hosting-intro.html) of how to setup a similar production environment for hosting a static website.

## Creating the deployment pipeline

With the production environment setup, we can create our deployment pipeline using Snap CI. Snap CI integrates with GitHub, so when you first try to access it, you will be redirected to GitHub to authorize the application to access information about your repositories.

Once logged in to Snap CI, to create the project you must choose which GitHub repository will be the trigger.

![choose GitHub repository](/assets/images/screenshots/aws-static-website-deploy/choose-github-repository.png){: .screenshot .big}

In our case, we click the + Add button on the devops-book-site repository. You will not see that project listed unless you have a fork on your GitHub account.

Snap CI will try to detect which type of application you have in your repository to pre-configure a pipeline with default stages. For example, if you have a Rails application it will create a FASTFEEDBACK stage that will execute Bundler and some standard Rake tasks to run your automated tests.

In our case, Snap CI will detect our Gemfile file and allow us to choose which version of Ruby we want to use with Bundler. We will choose the default Ruby version 1.9.3 and we don't need to configure any databases.

![choose GitHub repository](/assets/images/screenshots/aws-static-website-deploy/samplestage.png){: .screenshot .big}

Next, we rename the SampleStage to “Deploy” to indicate that this stage deploys the website to AWS.

![choose GitHub repository](/assets/images/screenshots/aws-static-website-deploy/rename-stage-deploy.png){: .screenshot .big}

Next we need to define the commands to execute on each task:

![choose GitHub repository](/assets/images/screenshots/aws-static-website-deploy/commands-execute.png){: .screenshot .big}

The first task will run Bundler to install the required gems. We are using the [s3_website](https://github.com/laurilehmijoki/s3_website) gem to deploy our website to AWS, because it handles file uploads to our S3 bucket as well as CDN configuration for our CloudFront distribution on each deploy. Furthermore, it supports asset compression with GZIP and configuring cache headers to improve the static website performance.

The second task will run the command s3_website push with the --headless option – so the tool won't require any user interaction – and the --site option to describe which files are part of our static website. Once the tasks are configured, we need to define a few environment variables.

![choose GitHub repository](/assets/images/screenshots/aws-static-website-deploy/environment-variables.png){: .screenshot .big}

**For security reasons, you should not commit any sensitive information in plain text on your GitHub repository.** If you do, anyone can use your AWS credentials to spin up multiple AWS resources using your account and you will have to pay the bill at the end of the month. That is why we decided to configure this information as secure environment variables that will be available at deploy time without requiring them to be committed in the repository and without them being revealed to others in the stage configuration. To understand how they are used, you can look at the [s3_website.yml](https://github.com/dtsato/devops-book-site/blob/master/s3_website.yml) configuration file:

<pre>
  <code>
    s3_id: <%= ENV['AWS_ACCESS_KEY_ID'] %>
    s3_secret: <%= ENV['AWS_SECRET_ACCESS_KEY'] %>
    s3_bucket: <%= ENV['S3_BUCKET'] %>
  </code>
</pre>

Once you click Build Now your project will be created and, within a few moments, Snap CI will start executing your first build:

![choose GitHub repository](/assets/images/screenshots/aws-static-website-deploy/snap-ci-execute-build.png){: .screenshot .big}

While the build is running, you can click on the stage to see a real time log of how it is progressing. At the end of the execution, you will have a complete log of all the commands that were executed and their respective outputs:

![choose GitHub repository](/assets/images/screenshots/aws-static-website-deploy/logs-build.png){: .screenshot .big}

Success! We have configured a simple deployment pipeline consisting of a single stage that will deploy a static website to AWS every time a new commit is made to GitHub. For more complex applications, your pipeline will probably require more than one stage, so you can test it and get enough confidence before releasing it to production.

![choose GitHub repository](/assets/images/screenshots/aws-static-website-deploy/snap-ci-multiple-pipelines.png){: .screenshot .big}

As new commits or configuration changes are made, Snap CI will keep a history of your deployment pipeline execution, giving you end to end traceability from commit to deployment:

Conclusion

In this article we showed how to configure a simple deployment pipeline to implement continuous deployment of a static website to AWS using Snap CI. The source code is available on GitHub if you want to get more details about how we are using these tools.

Although our example is simple, it shows how easy it is to create and configure a deployment pipeline using Snap CI. Snap CI has many other features that were not covered in this article, such as: artifact promotion, pipelines with multiple stages, manual trigger for stages, support for integration pipelines to track GitHub branches, deploy to Heroku, and much more. You can explore the Snap CI [documentation](https://docs.snap-ci.com/) to learn more details and subscribe to their [blog](http://blog.snap-ci.com/) to keep up-to-date about the latest news and releases.

To better understand the concepts and practices presented in this article or to see a more complete and elaborate example, you can read the book [DevOps na Prática](https://casadocodigo.refersion.com/l/b84.1397), currently only available in Portuguese. The static website discussed in this article is live on [http://www.devopsnapratica.com.br](http://www.devopsnapratica.com.br/) and it has more information about the book's content (currently only available in Portuguese).

_This post was originally published on ThoughtWorks Insights [Continuous Delivery Channel](https://www.thoughtworks.com/insights/blog/continuous-deployment-static-website-aws-snapci)

 
Snap CI © 2017, ThoughtWorks
