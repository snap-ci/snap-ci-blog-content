---
layout: post
title:  "Different Ways to Structure Your Chef Code"
description: "The pros and cons of having your chef code with your code and in a separate repo"
date:   2016-07-05
author: Badri Janakiraman
index-image-url: screenshots/chef-code/structure-chef-code.jpg
index-image-alt: 'structure chef code'
keywords: chef, infrastructure as code, code organization, chef cookbook, ansible, git, continuous integration, continuous deployment
categories: CONTINUOUS-DELIVERY DEVOPS
---

![Structure your Chef code](/assets/images/screenshots/chef-code/structure-chef-code.jpg){: .graphic}

There are a few different ways to structure your Chef repositories and your application code. They each have some pros and some cons, so I wanted to explore a couple of the more common options and talk through the problems with each of them. I hope that reading this will either prompt you to tell how you organize your code, or to let us know about additional workarounds you've found to the problems I discuss.

For the scope of this post, I will restrict myself to using Git as the version control system and Chef for infrastructure automation. To the best of my knowledge, the issues discussed below should apply regardless of whether you're using Chef or Puppet or Salt Stack or Ansible or plain old shell-scripts. However, if I'm wrong and there is a fundamentally better way of solving the highlighted issues with another tool, I would love to hear that too.

With that said, feel free to substitute Chef cookbook with Puppet manifest or Salt formula or Ansible playbook and let me know if any of the following don't hold up.

### Chef code lives in the application repository

![SingleRepo](/assets/images/chef-organization/one_repo.png){: .graphic}


The first option is to keep the cookbooks that contain recipes for how to configure and deploy the service in the same repository as the code itself. Let's say that your application code and corresponding tests live at the top level of the repository the app and spec directories. Let's say the cookbooks and specs for those, such as they might exist, live in the cookbooks and test/integration directories.

##### Pro: Self-describing applications
This style of maintaining your Chef code has the advantage that the application code and the infrastructure code are proximal to each other. This proximity means that a quick scan of the repository shows the application's behavior and all the information needed to understand how and where it gets deployed. The run-time shape of the application and its statically defined behavior are equivalent and can all be taken in from the same snapshot of the repository.

##### Pro: Simpler deployments
Since the infrastructure automation scripts travel with the application, it is easy to have application changes and the requisite infrastructure changes deployed hand-in-hand. The infrastructure automation is available across branches and pull-requests. This means that even members of the team who only occasionally use these mechanisms can use the exact same automation mechanisms to verify their changes as the code that gets built and deployed from the master branch.

##### Con: more complex pipelines
On the flip side, building and deploying the application could get more complex. When changes to the app or spec directories are made, you might want to also run the specs and build a new application package. For changes in the cookbooks and test/integration directories, you might want to launch a Vagrant machine to test the cookbook changes using test-kitchen. You may not want to run your suite of test-kitchen integration tests when you make changes to your application logic. It is certainly possibly to structure your downstream deployment pipelines in this manner (using folder exclude glob patterns), but it is more complex to do so. In Snap, in the interest of simplicity, it is just not even possible to configure this sort of exclusion filter, though it is possible in [Go.CD](https://www.go.cd/).

##### Con: Duplicated configuration in multiple module systems

When an application is composed of many different modules, each service or component contains the cookbooks required to provision and deploy itself. One problem that comes up in this model is that if there's a shared resource that needs configuring, this might end up getting duplicated in the Chef cookbook in each repository. A common example is Apache or Nginx configuration. All the services in the ecosystem should ideally be configured in exactly the same manner - listening on the same ports, using the same cipher suites and certificates, with the same access and error logs, etc. However, in this model, this often results in duplication of the cookbook/recipes. For example, if a security patch were to be deployed, or a certificate replaced, it would require work in each individual repository to fix this issue.

### Chef code lives in its own repository

![SingleRepo](/assets/images/chef-organization/different_repos.png){: .graphic}

In contrast to the above model, the Chef cookbooks code could live in a different repository than the application code. In the simplest of cases, where there is a single monolithic service in the application, this results in twice as many repositories to manage. However, once the application reaches a certain threshold of complexity and has multiple components, the additional repository to store the cookbooks is not particularly onerous. However, even so, this model has its own pros and cons.

##### Pro: Centralized management of shared resources

The first benefit of this is that the problem of shared resources, such as the OpenSSL package version and Apache/Nginx configurations mentioned in the previous section can be centralized in this repository. When a package needs to be upgraded or a configuration change needs to be made, the actual edit needs to be made in exactly one repository. Fewer places to make a repeated change means that there are fewer chances to make a mistake, which is always a Good Thing™.

##### Pro: Natural place to define roles and environments

A chef-repo is a natural place to define things such as server roles and environments. Especially in a multiple module system, no single service is a good place to define server roles or environment definitions. This is because a single server might play multiple roles by having more than one service running on it. Similarly, an environment might have a single physical machine playing all the various roles - or it might have multiple different servers each playing a specific role - or multiple different load balanced servers playing a single role. The chef-repo provides a single point to look at the overall runtime definition and shape of an application composed of all its interacting services.

##### Con: Hard to make configuration changes in sync with code

In this model it is impossible to make changes to the code along with changes to the chef-repo. Let's say you make a change to service A that requires a corresponding change to the deployment scripts for it to work as expected. You could make the changes to your service under question first and then move on to working on your chef-repo. However, while you are working on the cookbooks in the chef-repo, which might take a couple of hours, your teammates might need to deploy service A. So this means that unless you made your changes to the service such that it could be deployed by an earlier version of the cookbooks in the repo, you might run into coordination issues. Of course, such issues can be resolved by having team practices and conventions, but the issue remains that when the changes to the application and its deployment scripts don't happen in sync, additional techniques or processes may be required.

### Alternatives

There are some alternatives to these options. At least in the Chef world, Berkshelf provides a mechanism for having a shared repository of cookbooks, but including only the ones you want for each service in each repository. A poor-man's version of including cookbooks in from a central repository can also be done by moving to using Git sub-modules. However, sub-modules present an additional complexity in and of themselves and it is worth examining if the cost is worth the benefits.

So based on these choices, how do you structure your cookbooks? Let me know in the comments below. I look forward to hearing other stories!

 
Snap CI © 2017, ThoughtWorks
