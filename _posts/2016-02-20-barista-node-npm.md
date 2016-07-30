---
layout: post
title:  "Barista Node NPM"
date:   2016-04-04
author: "Sean McLellan"
tags: ["Node.js", "NPM"]
---

On the road to a provider hosted app...
A lot more work to do, but I have it to the point where it's possible to install and run individual barista server instances with NPM


If you have NodeJS installed, simply install and start the server with

```bash
$ npm install barista-server -g
$ barista-server
Barista listening at http://:::8000
```

then, simply point a browser to http://localhost:8000/fiddle
To uninstall:

```bash
$ npm uninstall barista-server -g
```

No NodeJS installed? See [this](https://nodejs.org/en/download/package-manager/#windows)


Github Repo:
https://github.com/BaristaLabs/barista-server