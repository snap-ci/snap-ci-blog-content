---
layout: post
title:  "A more shell-like environment to run your build"
description: Snap's stages currently provide an isolated environment around every command that you run in them.
date:   2014-11-04
author: Akshay Karle
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, snap shell, heroku
categories: announcements
---

Snap's stages currently provide an isolated environment around every command that you run in them. This is done by executing each of your build commands in their own shell. What this means though is that any [internal commands](http://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html#Bourne-Shell-Builtins) that you run in that command don't impact any following commands.

While isolation is good, this behaviour was often confusing. We heard from quite a few of you who logged support tickets wondering why your attempts at doing things like *cd* and *export* did not take effect and work like they did on your machine.

We thought it was perfectly reasonable that you would expect commands in your build environment to work as they do on your local machine. So we have decided to change the behaviour in Snap to better match your expectations!

> This change may affect your existing builds if you are changing the state of your shell by using the internal commands like *cd*, *export* or *exit*. If you want to preserve the old behaviour, an easy way to do so is to just run these internal commands in a [subshell](http://www.gnu.org/software/bash/manual/html_node/Command-Grouping.html).

Following are some sample commands that you might have to change in your build configuration:

#### Changes with the *cd* command:

Changing the directory in one command will now change for all subsequent commands. Lets say you had a stage like:

```bash
$ cd some_dir && run_specs
$ run_something_else
```

Today, *run_something_else* runs in the *$SNAP_WORKING_DIR* and not in *some_dir*. But once we deploy the changes that persist the session across the entire stage the working directory set in one command will be present in the next command. So, you may want to do something like the following to make sure your build doesn't fail:

```bash
$ (cd some_dir && run_specs)
$ run_something_else
```

Running a command within `()` ensures that it runs in a subshell and so the subsequent command will run in the $SNAP_WORKING_DIR just as before.

#### Changes with the *exit* command:

The *exit* command will now cause your session to end - and thus kill your stage! We've had cases where users used *exit* to continue or fail their builds based on the exit status. Let's look at the following example used for the *heroku run* command:

```bash
$ buffer_file=/tmp/last_heroku_run; heroku run --app 'APP_NAME' 'YOUR_COMMAND; echo $?' | tee $buffer_file; exit `tail -1 $buffer_file`
$ heroku maintenance:off --app 'APP_NAME'
```

Today, *heroku maintenance:off --app 'APP_NAME'* will run if the heroku run command would do *exit 0*. But once we push the change to run each stage in a shell, this will always fail even with a *exit 0* since *exit* will now actually exit the current session. The easiest way to mitigate such a case is to run the exit command in a subshell. You can change your stage to be something like the following:

```bash
$ buffer_file=/tmp/last_heroku_run; heroku run --app 'APP_NAME' 'YOUR_COMMAND; echo $?' | tee $buffer_file; (exit `tail -1 $buffer_file`)
$ heroku maintenance:off --app 'APP_NAME'
```

#### Changes with the *export* command:

The *export* command will now cause environment variables to be set for all the commands in the stage. Let's look at an example again:

```bash
$ export RAILS_ENV=production; bundle exec rake assets:precompile
$ bundle exec rake spec:smokes
```

This will work just fine in today's case as the RAILS_ENV will be unset when running the *bundle exec rake spec:smokes* command. But when the bash session will be persisted across the stage, you may have to unset the RAILS_ENV variable before running the smoke tests. One way to make sure that the exported variable is not available to subsequent command is to run in it a subshell:

```bash
$ (export RAILS_ENV=production; bundle exec rake assets:precompile)
$ bundle exec rake spec:smokes
```

These are just a few examples for you to get an idea of the impact the changes may have on your build. You can make these changes right away and your build should continue to work just fine. In case you don't get a chance to update your configuration, we will run a migration that make sure your builds continue to work exactly as they do today. This change will go in effect on Wednesday, Nov 5th, 2014 around evening UTC. If you have any doubts or problems with the changes coming in please feel free to comment below or get in [touch with us]({{ site.link.contact_us }}).

 
Snap CI © 2017, ThoughtWorks
