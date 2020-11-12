---
layout: post
section-type: post
date: 2020-11-12
lastmod: 2020-11-12
title: Building a multiplayer JS game with Colyseus.js in 20 minutes
slug: multiplayer-js-game-workshop
url_slug: multiplayer-js-game-workshop
category: tutorials
tags:
  - code
  - javascript
  - game
  - tutorial
postdescription: Using TypeScript along with Colyseus.js and Phaser.js, we will
  create a very simple multiplayer browser game in less than 30 minutes.
  Originally uploaded as a Hack Club Workshop.
titletag: Building a multiplayer JS game with Colyseus.js and Phaser.js in 20 minutes
description: Using TypeScript along with Colyseus.js and Phaser.js, we will
  create a very simple multiplayer browser game in less than 30 minutes.
---
In this tutorial, I will explain the basics of creating a multiplayer browser game with the popular JS frameworks [Colyseus.js](https://docs.colyseus.io/) and [Phaser.js](https://photonstorm.github.io/phaser3-docs/).

## Setting up the server

The easiest way to get started is by starting a new TypeScript Repl at <https://repl.it/languages/typescript> (if you would prefer to use a local environment instead, go ahead). I would recommend renaming it to something like "My Multiplayer Game" from the dropdown at the top so you can easily find it again.

<!--TODO install if local env
First, we need to install the Colyseus server framework. Navigate to the "packages" tab in your new Repl project and search for "colyseus". Once you find it, select it and click on the plus icon to add it to your project.

![Searching for the Colyseus package](/img/uploads/addcolyseuspackagesearch.png "Searching for the package")

![Adding Colyseus](/img/uploads/colyseuspackageaddd.png "Adding Colyseus")-->

### Importing the packages

There are several packages we need to import into our gameserver, including Colyseus (the framework we'll use for networking), Express (a package we'll use for setting up the interface between the game and server), and CORs (a package that will configure our server so that the game can access it). Let's do that first by adding these lines to the top of `index.ts`:

```
import http from "http"
import express from "express"
import cors from "cors"
import { Server } from "colyseus"
import { monitor } from "@colyseus/monitor"
```


