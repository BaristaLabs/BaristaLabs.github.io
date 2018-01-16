---
layout: post
title:  "Announcing BaristaCore Availability on NuGet"
date:   2018-01-08
author: "Sean McLellan"
tags: ["Barista", "Chakra", "NuGet"]
---

I'm happy to share with you the availability of BaristaCore preview on NuGet.

Before I get into what is included with this release, the bits are available here: https://www.nuget.org/packages/BaristaCore/1.0.1-beta07

If you've been following along, Barista is a Functions-as-a-Service platform written in .net originally geared towards SharePoint development. BaristaCore is a grounds-up reimplementation of Barista that decouples SharePoint from the equation and leverages .Net Core as the foundation. That in itself has let Barista expand into being a cross-platform toolkit, but we didn't stop there.

JavaScript has evolved rapidly since the inception of Barista and while Jurassic was a great JavaScript engine implemented in pure managed code, it lacked the improvements that modern JavaScript development incorporates, things like async/await and a native module system. We also felt that its performance characteristics and sandboxing could be improved upon. Once Microsoft open sourced their Chakra engine, the same JavaScript engine that powers the Microsoft Edge engine, we knew that incorporating this engine was a must. Thus, a native/managed layer, that can be used in cross-platform scenarios, was developed and the ChakraCore engine is now part of BaristaCore and this preview release.

We feel that we're just scratching the surface of the capabilities of what ChakraCore can provide to Barista. Right now, in the preview, we have things like TypeScripe transpilation and React-based Server-Side implemented in ways that reduce the time that the engine needs to parse the JavaScript libraries, but there's alot more that we can do. 

The BaristaCore engine facilicates creating modules out of .Net based code using a runtime projection system. This system automatically converts .Net based type hierarchies into true JavaScript prototypical objects.

In the future, We're looking at such as time-travel debugging, generating .d.ts definitions out of projected types, and improving BaristaFiddle to be a more-full-featured IDE using the Monaco Editor -- the same editor that powers VS Code.

Right now, please feel free to explore the preview bits and submit issues as you find them. There are also a few wiki pages that guide incorporating BaristaCore into your own .Net core based application that you can find here: https://github.com/BaristaLabs/BaristaCore/wiki

We're really excited to be on this journey and hope you come along as we try to blur the lines between brower technologies, JavaScript Engines, Machine Learning and microservices.

Thanks,

Sean McLellan