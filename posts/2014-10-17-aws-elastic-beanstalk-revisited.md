---
layout: post
title:  "AWS Elastic Beanstalk: Revisited"
description: You can now use Snap's Elastic Beanstalk Deploy recipe. Have a look at the AWS getting started guide for steps on how to create a Beanstalk application.
date:   2014-10-17
author: Akshay Karle
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, AWS, elastic beanstalk, pipeline configuration
categories: announcements feature deployments
---

Last year, we had [demonstrated a way to deploy your application to Elastic Beanstalk]({% post_url 2013-10-21-aws-elastic-beanstalk %}). This involved creating a zip of your application and running a bunch of AWS commands. Kinda fell short of the kind of ease of use we aspire to and are known for.

Today I'm excited to announce a new and much improved way to do deployments to the beanstalk. You can now use Snap's [Elastic Beanstalk Deploy recipe](http://docs.snap-ci.com/deployments/aws-deployments/#using-elastic-beanstalk-to-deploy-to-aws) to do this.

You'll need to setup an application in AWS Beanstalk. Have a look at the [AWS getting started guide](http://aws.amazon.com/elasticbeanstalk/getting-started/) or our [previous blog]({% post_url 2013-10-21-aws-elastic-beanstalk %}) for steps on how to create a Beanstalk application.

> *TIP:* When creating the Elastic Beanstalk application on AWS, select a `Sample Application` and later when deploying through Snap supply a S3 bucket.

Once you have setup the application on AWS you can deploy from Snap with the following 3 simple steps:

* Add an Elastic Beanstalk Deploy stage to your pipeline configuration.
* Provide the Application name, Environment name, the S3 bucket and AWS credentials to the stage.
* Hit save and watch your changes get deployed to Elastic BeanStalk.



## Add a Elastic Beanstalk Deploy stage to your pipeline configuration

Visit your pipeline configuration edit page and select the add new stage. Select the `Elastic Beanstalk Deploy` recipe from the Deploy category.

![Add Stage]({% asset_path screenshots/aws-elastic-beanstalk-revisited/add-stage@2x.png %}){: .screenshot .big}



## Enter the Application name, Environment name, the S3 bucket and AWS credentials to the stage.

You can find the Application name and Environment name from the Elastic Beanstalk dashboard in the AWS console.

![AWS console for Application name]({% asset_path screenshots/aws-elastic-beanstalk-revisited/elastic-beanstalk-dashboard@2x.png %}){: .screenshot .big}

To deploy your application from Snap you also need to create a S3 bucket where snap can push the zipped app for Elastic beanstalk to fetch. Once you have created the s3 bucket enter the name of the S3 bucket in the `snap-deploy` command alongwith the Application name and the Environment name. You may also need to change the AWS region option, the default is us-east-1 (N. Virginia).

![enter credentials]({% asset_path screenshots/aws-elastic-beanstalk-revisited/enter-stage-data@2x.png %}){: .screenshot .big}


Have a look at our [docs](http://docs.snap-ci.com/deployments/aws-deployments/#using-elastic-beanstalk-to-deploy-to-aws%23command-line-usage-of-snap-deploy-for-opsworks-deployments) to get more details on the different options available for Elastic Beanstalk.



## Hit save and watch your changes get deployed to Elastic BeanStalk

![Snap deploy logs]({% asset_path screenshots/aws-elastic-beanstalk-revisited/deploy-console-logs@2x.png %}){: .screenshot .big}

![AWS console for Application name]({% asset_path screenshots/aws-elastic-beanstalk-revisited/elastic-beanstalk-dashboard-deployed@2x.png %}){: .screenshot .big}

As always, I would love to hear if you have any comments or tips to make this experience even better. Leave your comments below or [contact us]({{ site.link.contact_us }}) if you have any questions.

 
Snap CI © 2017, ThoughtWorks
