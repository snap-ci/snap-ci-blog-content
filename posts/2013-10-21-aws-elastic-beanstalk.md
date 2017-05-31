---
layout: post
title:  "Deploying to AWS using Elastic Beanstalk"
description: Snap now has the AWS CLI installed on all build boxes, allowing you to perform a number of AWS operations including deploying to AWS using Elastic Beanstalk
date:   2013-10-21
author: Akshay Karle
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, AWS, elastic beanstalk, continuous deployment
categories: AWS Elastic-Beanstalk Deployments
---

Snap now has the [AWS CLI](http://aws.amazon.com/cli/) installed on all build boxes. This allows you to perform a number of AWS operations including deploying to AWS using [Amazon's Elastic Beanstalk](http://aws.amazon.com/elasticbeanstalk/).

To deploy to AWS using Elastic Beanstalk you need to perform the following steps:
* Create an Elastic Beanstalk application.
* Create an environment where you wish to deploy your application.
* Create a S3 bucket where you can store your application to be deployed.
* Create an application version to deploy.

## Prerequisites for Deploying to Snap

In order to start using Snap for AWS deployments, we first need to setup an [Elastic Beanstalk application and environment](https://console.aws.amazon.com/elasticbeanstalk/home) from the AWS console as shown in the figures below:

![elastic beanstalk home]({% asset_path screenshots/aws-elastic-beanstalk/elastic-beanstalk-home.png %}){: .screenshot}

Click on *Create New Application* to get started and follow the sequence of steps below to set up an application.

![create new application]({% asset_path screenshots/aws-elastic-beanstalk/application-info.png %}){: .screenshot}

![environment type]({% asset_path screenshots/aws-elastic-beanstalk/environment-type.png %}){: .screenshot}

![application version]({% asset_path screenshots/aws-elastic-beanstalk/application-version.png %}){: .screenshot}

![environment info]({% asset_path screenshots/aws-elastic-beanstalk/environment-info.png %}){: .screenshot}

Note that if your application uses [RDS](http://aws.amazon.com/rds/) you will have to create a RDS instance when creating your environment. You can view the steps for creating the RDS instance [here](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_Ruby.rds.html). If you are using any other databases please ensure that you have them installed and configured on the environments you are deploying to.

![additional resources]({% asset_path screenshots/aws-elastic-beanstalk/additional-resources.png %}){: .screenshot}

![configuration details]({% asset_path screenshots/aws-elastic-beanstalk/configuration-details.png %}){: .screenshot}

![review information]({% asset_path screenshots/aws-elastic-beanstalk/review-information.png %}){: .screenshot}

We will also need to create a [S3 bucket](https://console.aws.amazon.com/s3/home) where you can store your application versions to deploy:

![create s3 bucket]({% asset_path screenshots/aws-elastic-beanstalk/create-s3-bucket.png %}){: .screenshot}

## Deploy your app from Snap

Once you've created a application and an environment using Elastic Beanstalk and created a S3 bucket to store the application versions, you can configure your project on Snap to start deploying to AWS. To deploy to AWS we will be adding a stage named *Deploy* to your build pipeline that does the following:

* Create a zip file of your current build
* Upload the zip file to a S3 bucket for deployment
* Create a new application version to deploy
* Update the environment to use the new application version

To configure your project edit your build plan from the project configuration page as shown below:

![build plan edit]({% asset_path screenshots/aws-elastic-beanstalk/build-plan-edit.png %}){: .screenshot}

Next click on ADD NEW and select Custom stage. Enter *Deploy* as the stage name and enter the following commands:

```bash
git checkout .
zip -r "APP_NAME.zip" * .*
aws elasticbeanstalk delete-application-version --application-name "APP_NAME" --version-label `git rev-parse --short HEAD` --delete-source-bundle
aws s3 cp APP_NAME.zip s3://S3_BUCKET_NAME/APP_NAME-`git rev-parse --short HEAD`.zip
aws elasticbeanstalk create-application-version --application-name "APP_NAME" --version-label `git rev-parse --short HEAD` --source-bundle S3Bucket="S3_BUCKET_NAME",S3Key="APP_NAME-`git rev-parse --short HEAD`.zip"
aws elasticbeanstalk update-environment --environment-name "ENVIRONMENT_NAME" --version-label `git rev-parse --short HEAD`
```

Next click on the Environment Variables tab and add the following [environment variables required by AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#config-settings-and-precedence):

* Key: AWS_ACCESS_KEY_ID      Value: YOUR_AWS_ACCESS_KEY_ID
* Key: AWS_SECRET_ACCESS_KEY  Value: YOUR_AWS_SECRET_ACCESS_KEY
* Key: AWS_DEFAULT_REGION     Value: YOUR_AWS_DEFAULT_REGION

Click Add to create the Deploy stage.

Now, there may be a couple of things here that may not be immediately obvious. For example, we perform a git checkout as the first step before zipping the application. This is done because Snap symlinks its own database.yml for Rails builds. If you deploy using that database.yml your application will fail as we don't define a production configuration in the one that we create for you. The git checkout resets your working directory on the build box.

Once this is done, we create an application archive.

The next thing you might notice is that we first delete any existing application versions with the same version label before creating a new application version with that label. This is done to ensure that you deploy the current build and not any previous versions.

Finally, the application archive's contents are used to update the new version of the application.

Now that you have the Deploy stage added to your pipeline, click the Save and Re-run button. Wait for the pipeline to complete and voila! You can now view your [Elastic Beanstalk dashboard](https://console.aws.amazon.com/elasticbeanstalk/home) to see the status of your newly deployed application.

![elastic beanstalk dashboard]({% asset_path screenshots/aws-elastic-beanstalk/elastic-beanstalk-dashboard.png %}){: .screenshot}

For more information see [AWS CLI reference](http://docs.aws.amazon.com/cli/latest/reference/), [Elastic Beanstalk](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html).

 
Snap CI © 2017, ThoughtWorks
