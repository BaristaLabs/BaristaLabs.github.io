---
layout: post
title:  "Barista-Node update"
date:   2014-10-13
author: "Sean McLellan"
tags: ["Barista Node"]
---

Some under-the scenes changes to barista-node.

Unfortunately, sandcastle has some issues of its own, so I built a custom solution.

For maximum isolation (until node 0.11.x is released, maybe beyond) the best way to provide for isolated instances is to 1) use separate node processes to execute scripts and 2) use the vm.runInNewContext method. Unfortunately this causes some problems.

Once multiple node processes are involved, there needs to be some sort of IPC involved. Sandcastle uses stdio which I think is one of the issues . in my custom implementation I went with a signal.io (websockets) approach. In a nutshell, incoming requests to the API endpoint send messages to a worker process hub. the hub picks a worker process from the queue and sends it a message to execute a script. When the worker process is complete, it signals the hub which in turn signals the originating endpoint and provides the response. The hub monitors the worker processes so that if they don't respond in a pre-determined time, it kills the process and recreates it. Future functionality will allow for CPU monitoring and Memory limits on the worker processes.

The second issue is that the current Node vm.runInNewContext fails on child async calls -- there's a module called contextify which helps solve this.

It all looks something like this.

![alt text](/img/2014-10-13-barista-node-update-01.png "Design")

There's some nice benefits to this approach

1) Worker processes don't need to live on the same host, so long as they connect to the socket.io based hub. This will allow barista to scale the number of workers.
2) Multiple web-facing hosts can also connect to the "master" pool that drives the worker pool. this allows for multiple web-facing instances.