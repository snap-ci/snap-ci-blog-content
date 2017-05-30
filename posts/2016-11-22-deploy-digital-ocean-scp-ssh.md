---
layout: post
title:  "Snap CI Deployments to DigitalOcean via SCP and SSH"
description: "How to get your Snap CI builds deployed on DigitalOcean Droplet"
date:   2016-11-22
author: Ankit Srivastava
index-image-url: screenshots/from-support-desk/from-support-desk.jpg
index-image-alt: 'digital ocean deploy'
keywords: deploy software, continuous deployment, software deployment, continuous delivery, devops
categories: SUPPORT
---

Currently, Snap [supports](https://docs.snap-ci.com/deployments/heroku-deployments/) Heroku and [AWS](https://docs.snap-ci.com/deployments/aws-deployments/) deployments. DigitalOcean is a cloud infrastructure provider that has recently gained popularity, and lately we've gotten many requests for Snap CI integration with it.

(If you're unfamiliar with DigitalOcean, you can read more about it [here](https://www.digitalocean.com/help/))

While a DigitalOcean integration is in our backlog, I would like to share my experience working on a sample node project and deploying it onto DigitalOcean Droplet.

I’m using a *Hello world* example below for testing on Snap CI and deploying it on DigitalOcean.
GitHub repo url can be found here: [https://github.com/ankitsri11/node_project_digitalocean](https://github.com/ankitsri11/node_project_digitalocean)

#### Hello.js file content

```
#! /usr/bin/env nodejs
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(8080, 'localhost');
console.log('Server running at http://localhost:8080/');
```

#### Setting up DigitalOcean Droplet for Ubuntu machine:

a. Create a deploy user with sudo access
```
$ adduser deploy
$ gpasswd -a deploy sudo
$ su - deploy
```

b. Installation of required packages
```
$ apt-get -y install node
$ apt-get -y install nodejs-legacy
$ apt-get update
$ apt-get -y install npm
$ npm install -g pm2
```

#### Setup ssh authentication for deploy user:

a. Generate ssh key pair locally
```
$ ssh-keygen -t rsa
```

You can also refer our [help docs](https://docs.snap-ci.com/getting-started/ssh-keys/) for ssh key generation.

b. Create *authorized_keys* file under /home/deploy/.ssh folder for deploy user (in DigitalOcean server) and add the locally generated public key to that file.

#### Setting up Snap CI for build, test and deployment of nodejs code

For this, let's just create two stages: one for build+test and another one for DigitalOcean deployment. If required, we can always create more.

a. In Stage 1, I’m just running *hello.js* file and testing if I get the desired output.
```
$ chmod +x ./hello.js
$ nohup node hello.js &
$ sleep 20
$ curl http://localhost:8080
```

b. Stage 2 or deployment stage:
```
$ scp hello.js deploy@droplet_ip:/home/deploy/node_project/
$ ssh deploy@droplet_ip 'pm2 start /home/deploy/node_project/hello.js'
```

c. Add locally generated private key as a secure file and click on save.

![save-file](/assets/images/screenshots/digital-ocean/add-new-file.png){: .screenshot .big}

In this example I'm using a sample *hello world*, pushing the hello.js file, and deploying to DigitalOcean Droplet. Ideally, you should generate all the deploy files [as an artifact](https://docs.snap-ci.com/pipeline/#artifact) and then use *scp* command for uploading. You can also store your deploy script in your GitHub repository.

Hope this has been helpful. If you have questions, please comment below.
