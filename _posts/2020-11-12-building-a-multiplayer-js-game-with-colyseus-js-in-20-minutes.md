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

We will be using TypeScript for the server, as it is best for Colyseus. If you are not familiar with TypeScript, it is very similar to Node.js but requires you to specify types for variables and also lets you use some advanced/modern features that are unavailable in basic Node.js. Usually, you should be able to just write standard JS code and get away with it in TypeScript.

## Setting up the server

The easiest way to get started is by starting a new TypeScript Repl at <https://repl.it/languages/typescript> (if you would prefer to use a local environment instead, go ahead). I would recommend renaming it to something like "My Multiplayer Game" from the dropdown at the top so you can easily find it again.

### Importing the Packages

First, we need to install the Colyseus server framework and other packages. To specify them, add a file name `package.json` to your project with the following contents:

```json
{
  "private": true,
  "name": "myMultiplayerGame",
  "version": "0.1.0",
  "main": "index.ts",
  "scripts": {
    "start": "ts-node-dev index.ts",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "devDependencies": {
    "@types/cors": "^2.8.6",
    "@types/express": "^4.17.1",
    "ts-node": "^8.1.0",
    "ts-node-dev": "^1.0.0-pre.63",
    "typescript": "^3.4.5"
  },
  "dependencies": {
    "@colyseus/monitor": "^0.12.2",
    "@colyseus/social": "^0.10.9",
    "colyseus": "^0.14.0",
    "cors": "^2.8.5",
    "express": "^4.16.4",
    "express-jwt": "^5.3.1"
  }
}

```

There are several packages we need to import into our gameserver, including Colyseus (the framework we'll use for networking), Express (a package we'll use for setting up the interface between the game and server. It allows us to define "routes", which tell the server how to respond when the user requests a certain URL), and CORs (a package that will configure express so that the game can access it). Let's do that first by adding these lines to the top of `index.ts`:

```javascript
import http from "http"
import express from "express"
import cors from "cors"
import { Server } from "colyseus"
import { monitor } from "@colyseus/monitor"
```

### Starting the Server

Now, we need to write a few lines to set up the game server:

```javascript
const port = 3000
const app = express() // This line creates a new Express app

app.use(express.json()) // This line tells express to interpret incoming requests as JSON, which makes it easy for us to understand and interact with the requests.

const server = http.createServer(app) // here, we initialize a server using our express app.
const gameServer = new Server({ server }) // This line adds Colyseus, the game framework, to our Express server.

app.use('/colyseus', monitor()) // This sets up a route allowing us to view all the Colyseus data in real-time from a browser. We'll use it later.

app.get('/', (req: express.Request, res: express.Response) => {
  res.send("Hello, game server!") // Respond with "Hello, game server!" when the user visits the server.
})

gameServer.listen(port) // Finally, we start the server by listening to incoming requests.
console.log(`Listening on ws://localhost:${ port }`)

```

