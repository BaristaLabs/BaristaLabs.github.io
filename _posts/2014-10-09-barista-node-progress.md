---
layout: post
title:  "Barista-Node progress"
date:   2014-10-09
author: "Sean McLellan"
tags: ["Barista Node"]
---

At the end of the day yesterday, I have the base fiddle working, auto-complete, syntax checking... next up will be to add tabs additional keyboard shortcuts and more...


But first, there's some shortcomings in the js sandbox... so the quest for a true sandbox continues.

Built-up requirements:

1. Must be safe as possible. I.e. Run untrusted code without being able to break out of the sandbox and get at the os. Non-leaking is 
2. Must be able to be constrained or limited. A must have is ability to set a max script execution time, best case is to limit or at least monitor memory, throttle or restrict CPU/Network of the script.
3. Must have ability to expose custom globals, preferably a mixin of node + custom methods.
4. Should be based on v8. This is to allow for in-browser debugging of executing scripts (as previously prototyped https://github.com/Oceanswave/V8SignalRDebugging) If not v8, then should be a javascript engine implementation that allows for runtime debugging.


Going down the rabbit hole of creating my own sandbox,  I spent much of the day creating an npm package that builds v8 on preinstall. That work is here https://github.com/Oceanswave/v8-node 

more needs to be done to wrap and expose the v8 objects to node.


Learning a lot about node/npm/modules/node-gyp remembering my c++ and how to build node addons. With some of this knowledge there might be a way to reuse the node v8 impl within a node addon without rebuilding the wheel. This seems to be the hottest thread at the moment, and the focus tomorrow.

Update 2016-07-30: Repository is here: [https://github.com/BaristaLabs/barista-core](https://github.com/BaristaLabs/barista-core)