---
layout: post
title:  "Barista Core: Progress report"
date:   2016-11-14
author: "Sean McLellan"
tags: ["Barista", "Chakra"]
---

A while back, we embarked on a journey to modernize Barista. If you're new to this site, Barista is an interactive environment that lets you seamlessly
connect scripts with compiled code (in .NET) to enable fast and flexible development of REST services. The environment includes powerful and efficient libraries
for integrating with software like sharepoint, lucene.net, and provides built-in capabilities to support document generation (PDF, excel, HTML) that can be used from any .NET language.
Barista also provides a feature-rich interactive development environment for rapid development.

Some of the desires we had were to make Barista a cross-platform solution. Being cross-platform which would allow us to leverage cloud-based computing and commodity servers to provide 
Barista as a SaaS to the community at large. Additionally, we wanted modernize the underlying script engine to take advantage of the latest standards and provide a much greater level of 
performance than we've been able to provide.

Today I'd like to announce some progress on this front. While we initialially started with Node.Js, leveraging the V8 engine within a sandboxed process, .Net Core has really ramped up and 
actually delivered on many of the promises. Additionally, the Chakra team (The engine that powers Microsoft Edge) has provided a cross-platform library that gives us a modern scripting
engine, with great performance and a wonderful API as a C++ Library.

Since the .Net Core/Chakra story has become very compelling, we've switched gears a bit and focused on developing a .Net Core based Barista middleware that uses p/invoke to interoperate
with the unmanaged ChakraCore library. This work is available [here](https://github.com/BaristaLabs/BaristaCore/). 

While there is a lot of work still left to be done, our unit tests are currently passing on both Windows, macOS, and Ubuntu 16.04.1.

​![alt text](/img/2016-11-14-barista-core-unit-tests-01.png "Passing Windows xUnit Tests")
​![alt text](/img/2016-11-14-barista-core-unit-tests-02.png "Passing macOS xUnit Tests")
​![alt text](/img/2016-11-14-barista-core-unit-tests-03.png "Passing Ubuntu 16.04.1 xUnit Tests")

This is very exciting to us and we look forward to making Barista the best rapid development environment we can.

> Update: Added Ubuntu 16.04.1 