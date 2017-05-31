---
layout: post
title:  "Deploying to OpsWorks in a Snap"
description: AWS OpsWorks is a service that allows you to create and manage these resources collectively in a stack.
date:   2014-10-15
author: Akshay Karle
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, opsworks, deployments, AWS
categories: announcements feature deployments
---

AWS allows creating and managing a number of resources such as the EC2 instances, volumes, databases, etc. [AWS OpsWorks](http://aws.amazon.com/opsworks/) is a service that allows you to create and manage these resources collectively in a stack. A stack consists of multiple application layers composed of different AWS resources. Each layer has one or more applications each of which corresponds to an App Server where your code is deployed. To know more about OpsWorks stacks, layers and apps have a look at their [docs on OpsWorks](http://docs.aws.amazon.com/opsworks/latest/userguide/welcome.html). In this blog, I will demonstrate how you can deploy an application using OpsWorks.

If you want to know how to setup an application using OpsWorks take a look at this [getting started guide for OpsWorks](http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-simple.html) which guides you through deploying a simple PHP application. I will be extending this tutorial to deploy the same application from Snap. You can [fork the sample php application](https://github.com/amazonwebservices/opsworks-demo-php-simple-app) from GitHub. Once you have setup a stack on OpsWorks you can deploy the application from Snap instead of deploying manually from the OpsWorks console. To get started, setup your repository on Snap. Setting up a deployment to the OpsWorks application involves the following steps:

* Add a OpsWorks deployment stage to your pipeline configuration.
* Enter the OpsWorks ID and AWS credentials to the stage.
* Hit save and watch your changes get deployed to OpsWorks.

Let us go through each step a little in detail now.



## Adding a OpsWorks deployment stage:

Visit your pipeline configuration edit page and select the add new stage. Select the `Basic OpsWorks` recipe from the Deploy category.

![Add Stage]({% asset_path screenshots/aws-opsworks/add-stage@2x.png %}){: .screenshot .big}



## Enter the OpsWorks ID and AWS credentials to the stage:

You can find the OpsWorks ID of your application from the Apps tab in the AWS console.

![Application ID]({% asset_path screenshots/aws-opsworks/opsworks-id@2x.png %}){: .screenshot .big}

You need to paste this ID in the `snap-deploy` command that does the deploy.

![OpsWorks ID]({% asset_path screenshots/aws-opsworks/add-opsworks-id@2x.png %}){: .screenshot .big}

You may also choose to migrate the database using the `--migrate` argument or wait for the deployment to complete using the `--wait` argument. Waiting for a deployment to finish is generally a good idea since it gives a sense of whether the deployment was successful or it failed for some reason. Since this repository doesn't have any database I will remove the `--migrate` argument but keep the `--wait` argument. Have a look at our [docs](http://docs.snap-ci.com/deployments/aws-deployments/#using-opsworks-to-deploy-to-aws%23command-line-usage-of-snap-deploy-for-opsworks-deployments) to get more details on the different options available for OpsWorks.

You can find the AWS credentials [here](https://console.aws.amazon.com/iam/home?#security_credential). You need to paste these credentials in the 2 secure environment variables placeholders.

![AWS credentials]({% asset_path screenshots/aws-opsworks/add-aws-creds@2x.png %}){: .screenshot .big}



## Hit save and watch your changes get deployed to OpsWorks:

![Deployed]({% asset_path screenshots/aws-opsworks/deployed@2x.png %}){: .screenshot .big}

![PHP app deployed]({% asset_path screenshots/aws-opsworks/php-deployed@2x.png %}){: .screenshot .big}



That's it, it takes 3 simple steps to setup a deployment to your OpsWorks stack from Snap. Isn't it simple? Hope you enjoy deploying from Snap! I would love to hear if you have any comments or tips to make this experience even better. Leave your comments below or [contact us]({{ site.link.contact_us }}) if you have any questions.

 
Snap CI © 2017, ThoughtWorks
