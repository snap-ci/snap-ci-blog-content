---
layout: post
title:  "Using LXD and cgroups"
description: "We’ll create a container and then constrain the number of processes that container can spawn so a rogue user can’t create a fork bomb inside of it!"
date:   2016-11-01
author: Kyle Olivo
index-image-url: screenshots/containers/lxd-cgroups.jpg
index-image-alt: 'using lxc and cgroups'
keywords: snap ci, lxd, lxc, containers, cgroups, vagrant box, linux, software deployment
categories: CONTAINERS
---


Let’s have a little fun with LXC containers and cgroups. We’ll create a container and then constrain the number of processes that container can spawn so a rogue user can’t create a fork bomb inside of it!

First, let’s create a vagrant box to serve as our playground for this activity. If you aren’t familiar with Vagrant, take a look at [their documentation](https://www.vagrantup.com/docs/) to learn how to install it.

```
$> vagrant init bento/ubuntu-16.04
...
$> vagrant up
...
$> vagrant ssh
```

Now let’s install the LXD package.

```
$> sudo apt-get install lxd
```

If you read the [previous blog post](https://blog.snap-ci.com/blog/2016/10/25/how-linux-containers-work/) you know that LXC consists of a set of tools that allows you to manage containers on a Linux system. [LXD](https://linuxcontainers.org/lxd/introduction/) is a daemon that allows management of [LXC](https://linuxcontainers.org/lxc/introduction/) containers through a REST interface either over the network or via a redesigned ‘lxc’ command line experience (which actually also communicates with the daemon via the REST API).

Next we’ll need to add ourselves to the ‘lxd’ group and initialize lxd.

```
vagrant@vagrant:~$ sudo usermod -a -G lxd vagrant
vagrant@vagrant:~$ exit
...
$> vagrant ssh
vagrant@vagrant:~$ groups
vagrant adm cdrom sudo dip plugdev lpadmin sambashare lxd
vagrant@vagrant:~$ sudo lxd init
Name of the storage backend to use (dir or zfs): dir
Would you like LXD to be available over the network (yes/no)? no
Do you want to configure the LXD bridge (yes/no)? yes
... # note: just hit enter on the questions about configuring the network
LXD has been successfully configured.
```

If you just type the ‘lxc’ command, you’ll see a list of subcommands.

```
vagrant@vagrant:~$ lxc
Usage: lxc [subcommand] [options]
Available commands:
       	config     - Manage configuration.
       	copy       - Copy containers within or in between lxd instances.
       	delete     - Delete containers or container snapshots.
       	exec       - Execute the specified command in a container.
       	file       - Manage files on a container.
       	help       - Presents details on how to use LXD.
       	image      - Manipulate container images.
       	info       - List information on LXD servers and containers.
       	launch     - Launch a container from a particular image.
       	list       - Lists the available resources.
       	move       - Move containers within or in between lxd instances.
       	profile    - Manage configuration profiles.
       	publish    - Publish containers as images.
       	remote     - Manage remote LXD servers.
       	restart    - Changes state of one or more containers to restart.
       	restore    - Set the current state of a resource back to a snapshot.
       	snapshot   - Create a read-only snapshot of a container.
       	start      - Changes state of one or more containers to start.
       	stop       - Changes state of one or more containers to stop.
       	version    - Prints the version number of this client tool.

Options:
  --all              Print less common commands.
  --debug            Print debug information.
  --verbose          Print verbose information.

Environment:
  LXD_CONF           Path to an alternate client configuration directory.
  LXD_DIR            Path to an alternate server directory.
```

  Many of the commands are quite intuitive. The most important subcommands for now are ‘launch’, ‘start’, ‘stop’, and ‘delete’. Let’s try to launch a container and connect to it.

```
vagrant@vagrant:~$ lxc launch ubuntu:
Creating irreplaceable-madie
Retrieving image: 100%
Starting irreplaceable-madie
vagrant@vagrant:~$ lxc list
+---------------------+---------+-----------------------+-----------------------------------------------+------------+-----------+
|        NAME         |  STATE  |         IPV4          |                     IPV6                      |    TYPE    | SNAPSHOTS |
+---------------------+---------+-----------------------+-----------------------------------------------+------------+-----------+
| irreplaceable-madie | RUNNING | 10.150.198.185 (eth0) | fd63:a75e:9d73:78ff:216:3eff:fe94:b32c (eth0) | PERSISTENT | 0         |
+---------------------+---------+-----------------------+-----------------------------------------------+------------+-----------+
```

This command indicates that we want to launch an ubuntu container, and since the name after the colon was omitted, we’ll be given the latest version of the ubuntu container. The container’s name is generated and will be different for every container. This one happens to be called ‘irreplaceable-madie’. Now let’s look at some information about the container.

```
vagrant@vagrant:~$ lxc info irreplaceable-madie
Name: irreplaceable-madie
Architecture: x86_64
Created: 2016/08/22 04:59 UTC
Status: Running
Type: persistent
Profiles: default
Pid: 3852
Ips:
  eth0:	inet   	10.150.198.185 	vethEKODR3
  eth0:	inet6  	fd63:a75e:9d73:78ff:216:3eff:fe94:b32c 	vethEKODR3
  eth0:	inet6  	fe80::216:3eff:fe94:b32c       	vethEKODR3
  lo:  	inet   	127.0.0.1
  lo:  	inet6  	::1
Resources:
  Processes: 24
  Memory usage:
    Memory (current): 59.80MB
    Memory (peak): 166.42MB
  Network usage:
    eth0:
      Bytes received: 2.12kB
      Bytes sent: 1.44kB
      Packets received: 20
      Packets sent: 12
    lo:
      Bytes received: 264 bytes
      Bytes sent: 264 bytes
      Packets received: 4
      Packets sent: 4
```

Two items are worth noting here. First, we have the process id (pid) of the container (3852). Second, we also get a count of the number of processes currently running in the container (24). What we’re going to do is place an upper limit on the number of processes that can run, because we want to be able to prevent a fork bomb from running on our system. [Fork-exec](https://en.wikipedia.org/wiki/Fork%E2%80%93exec) is the mechanism used in Linux to create new processes, by chaining an unending chain of fork-execs together, we can [deny service](https://en.wikipedia.org/wiki/Denial-of-service_attack) to anyone that might actually want to use our container. We won’t prove that the fork bomb works, because that would likely crash your container and even your vagrant box (because the container has unconstrained access to the resources on the vagrant host it is running on). Let’s place a limit on the number of processes so that this is no longer possible.

Before we set this limit, let’s explore cgroups (control groups) a little. Cgroups are implemented as data structures within the Linux kernel, so we need a way to manipulate something that is running in the kernel. Linux provides a few mechanisms for reading/writing to the kernel. The first is the read-only interface of the /proc filesystem. If we ‘ls’ that directory just like any other filesystem, we see a list of files that are actually live representations of data from the kernel. For instance, let’s look at some information about our memory usage.

```
vagrant@vagrant:~$ cat /proc/meminfo
MemTotal:         500192 kB
MemFree:          244388 kB
MemAvailable:     410848 kB
Buffers:           17284 kB
...
```

By simply issuing the ‘cat’ command on any of these files, we can see what the kernel’s current understanding of the system is. Interestingly, there is a cgroups file in /proc as well.

```
vagrant@vagrant:~$ cat /proc/cgroups
#subsys_name   	hierarchy      	num_cgroups    	enabled
cpuset 	9      	3      	1
cpu    	4      	136    	1
cpuacct	4      	136    	1
blkio  	7      	136    	1
memory 	8      	191    	1
devices	10     	136    	1
freezer	3      	3      	1
net_cls	5      	3      	1
perf_event     	11     	3      	1
net_prio       	5      	3      	1
hugetlb	6      	3      	1
pids   	2      	137    	1
vagrant@vagrant:~$
```

It’s telling us what subsystems (also called controllers) are defined in the kernel. A subsystem is just a grouping of resources for accounting and control purposes. For instance, the ‘cpuset’ subsystem above controls individual CPU cores in the system, while ‘cpu’ controls cpu cycles, and ‘pids’ controls process information. The other information here isn’t critical for what we’re trying to accomplish, but this document is the best resource I’ve found for a really in-depth, yet digestible explanation of what cgroups are. It also provides descriptions of each of the subsystems listed above. With the above information from /proc/cgroups, we now know there is a subsystem called ‘pids’ which contains the information we need. We can combine this with the pid of our container (shown to be 3852 earlier) to gain more knowledge about the cgroups for our process.

```
vagrant@vagrant:~$ cat /proc/3852/cgroup
11:perf_event:/lxc/irreplaceable-madie
10:devices:/lxc/irreplaceable-madie/init.scope
9:cpuset:/lxc/irreplaceable-madie
8:memory:/lxc/irreplaceable-madie/init.scope
7:blkio:/lxc/irreplaceable-madie/init.scope
6:hugetlb:/lxc/irreplaceable-madie
5:net_cls,net_prio:/lxc/irreplaceable-madie
4:cpu,cpuacct:/lxc/irreplaceable-madie/init.scope
3:freezer:/lxc/irreplaceable-madie
2:pids:/lxc/irreplaceable-madie/init.scope
1:name=systemd:/lxc/irreplaceable-madie/init.scope
```

The /proc filesystem provides information about every process running in the system, so by simply looking in /proc/[YOUR_PID] we can learn a lot about the environment surrounding our process. But we only care about the cgroup information for now. By issuing ‘cat’ on this file, we see information in this format, id:subsystems:path. Our goal again is to control the number of processes, so let’s focus on the ‘pids’ line. Just as there is a read path (/proc) for kernel information, there is another filesystem path for manipulating data in the kernel (/sys). If you issue a ‘mount’ command, you can see the subsystems mounted on the filesystem.

```
vagrant@vagrant:~$ mount | grep cgroup
tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,mode=755)
cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/lib/systemd/systemd-cgroups-agent,name=systemd)
cgroup on /sys/fs/cgroup/pids type cgroup (rw,nosuid,nodev,noexec,relatime,pids)
...
```

Great! We can see where the pids subsystem is mounted on the filesystem, and which options were used to mount it. If we dive into that directory, we can find the file that manipulates the maximum number of processes available in our container. We just need to combine the subsystem mount point with the pids path we found for our specific process above (which was /lxc/irreplaceable-madie/init.scope). There is one ‘gotcha’ though, we need to disregard the ‘init.scope’ subdirectory and apply our cgroup change at the level of the container name itself.

```
vagrant@vagrant:~$ cat /sys/fs/cgroup/pids/lxc/irreplaceable-madie/pids.max
max
vagrant@vagrant:~$ sudo su -c 'echo "1000" >> /sys/fs/cgroup/pids/lxc/irreplaceable-madie/pids.max'
vagrant@vagrant:~$ cat /sys/fs/cgroup/pids/lxc/irreplaceable-madie/pids.max
1000
```

We saw that the previous value was ‘max’ and now we’ve reduced that to 1000. This should stop our rogue user from fork bombing our container!

```
vagrant@vagrant:~$ lxc info irreplaceable-madie | grep Processes
  Processes: 26
vagrant@vagrant:~$ lxc exec irreplaceable-madie -- perl -e 'fork while fork' \&
vagrant@vagrant:~$ lxc info irreplaceable-madie | grep Processes
  Processes: 26
```

It looks like nothing happened, and that’s great. The processes spawned out of control until they hit our limit, and then they were were terminated. At this point you may object to this test, after all, I never proved that the fork bomb worked when the limit was set to max. If you are feeling adventurous, you can try that now, but will likely lose control of the container and vagrant box and have to restart them (possible even through the virtualbox interface). Before we make that change, I have a confession. There is a much easier way to control cgroup limits than by editing the /sys/fs filesystem directly (thankfully).

```
vagrant@vagrant:~$ cat /sys/fs/cgroup/pids/lxc/irreplaceable-madie/pids.max
1000
vagrant@vagrant:~$ lxc config set irreplaceable-madie limits.processes 100
vagrant@vagrant:~$ cat /sys/fs/cgroup/pids/lxc/irreplaceable-madie/pids.max
100
vagrant@vagrant:~$ lxc config unset irreplaceable-madie limits.processes
vagrant@vagrant:~$ cat /sys/fs/cgroup/pids/lxc/irreplaceable-madie/pids.max
max
```

How did I know that I should provide ‘limits.processes’ as the value to the get and set subcommands? Well, by [reading the documentation](https://github.com/lxc/lxd/blob/master/doc/configuration.md) of course! So the ‘config’ subcommand to ‘lxc’ provides a nice interface to the cgroup filesystem, and with this we were able to change and remove the original process limit that we had placed on the container. At this point you could try the fork bomb again in the container if you really want to see what happens without limits. Warning though, it isn’t pretty! By the way, if you want to drop down into the container, you can do so like this.

```
vagrant@vagrant:~$ lxc exec irreplaceable-madie bash
root@irreplaceable-madie:~#
```

Now you are free to issue any command you want within the confines of the container you created.

So just to recap, we were able to launch an LXC container and set a limit on the number of processes that could be spawned inside of it using cgroups. We gained some additional insight into what the kernel is doing my looking at the /proc filesytem, and saw that we could manipulate the kernel in realtime using the /sys filesystem. Now I encourage you to spend some time trying to place other limits on the container. For instance, are you able to constrain the amount of memory or cpu available to it? And what are all those other lxc subcommands about? Go and explore!


_This post was originally published on [Kyle Olivo's Blog](http://kyleolivo.com/dev/2016/08/21/lxd-and-cgroups/)_
