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
In this tutorial, I will explain the basics of creating a multiplayer browser game with the popular JS frameworks [Colyseus.js](https://docs.colyseus.io/) and [Phaser.js](https://photonstorm.github.io/phaser3-docs/). Using these frameworks, you will be able to build on this game to create something very complex, if you want!

## How it works

Most browser-based games use what are called _websockets_ to communicate with a _game server_, a special kind of web server responsible for synchronizing each player's movements across every player's browser. Websockets are not like traditional HTTP requests, as they form a _persistent, 2-way connection_ with the server, allowing each server and client to send messages back and forth to each other. For example, if one player in the game moves or does some other game action (depending on the type of game), the game will immediately send that move to the server using a socket connection, where the server will process the move (and perform any validation necessary to prevent cheating), and send it back over a socket connection to all the other players, whose games will process the new data and update the first player's position, so that everyone can see when one player moves. 

Fortunately, the browser's [WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) is easy enough to use already, but for this tutorial we will be using a JavaScript package called [Colyseus](https://colyseus.io/) that handles the websocket connection and game state automatically, so we can just focus on the game itself.

As for the game itself, we will be using [Phaser.js](https://phaser.io/), another open-source package for making browser games. Phaser is a very large framework, with everything from physics engines to sound tools to animation utilities. In this tutorial, we will only be using a tiny fraction of its available features, but you may find its other features useful if you plan to continue expanding and improving this game.

We will be using the [TypeScript programming language](https://www.typescriptlang.org/docs/) for the server, as it is the recommended language for Colyseus. If you are not familiar with TypeScript, it is very similar to Node.js but requires you to specify types (such as string, number, or "any") for variables and also lets you use some advanced/modern features that are unavailable in basic Node.js. But usually, you should be able to just write standard JS code and get away with it in TypeScript.

## Setting up the server

The easiest way to get started is by starting a new TypeScript Repl at <https://repl.it/languages/typescript> (if you would prefer to use a local environment instead, go ahead). I would recommend renaming it to something like "My Multiplayer Game" from the dropdown at the top so you can easily find it again.

Now, there's one small configuration change we need to make so that the typescript compiles properly. I won't get into the details here, but you can delete everything in the `tsconfig.json` file and replace it with this (you can just copy and paste):

```
{
  "compilerOptions": {
    "outDir": "lib",
    "target": "es6",
    "module": "commonjs",
    "strict": true,
    "strictNullChecks": false,
    "esModuleInterop": true,
    "experimentalDecorators": true
  }
}
```

### Importing the Packages

There are several packages we need to import into our gameserver, including Colyseus (the server and networking framework that will allow us to make our game multiplayer), and Express (a package we'll use for setting up the interface between the game and server. It allows us to define "routes", which tell the server how to respond when the user requests a certain URL)

Now, we need to install the Colyseus server framework and other packages our game will use. To specify them, add a file named `package.json` to your project with the following contents (you can just copy and paste):

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
    "@types/express": "^4.17.1",
    "ts-node": "^8.1.0",
    "ts-node-dev": "^1.0.0-pre.63",
    "typescript": "^3.4.5"
  },
  "dependencies": {
    "@colyseus/monitor": "^0.12.2",
    "colyseus": "^0.14.0",
    "express": "^4.16.4"
  }
}
```

Now, let's start writing code!!!!

Open `index.ts` and add these lines to the top to import the packages we just added (along with some basic packages built into TypeScript/Node):

```javascript
import http from "http"
import path from "path"
import express from "express"
import { Server } from "colyseus"
import { monitor } from "@colyseus/monitor"
```

### Starting the Server

Now, we need to write a few lines to set up the game server. Add this to the rest of `index.ts`. Make sure you read the comments so you know what's going on!

```javascript
const port = 3000
const app = express() // This line creates a new Express app

app.use(express.json()) // This line tells express to interpret incoming requests as JSON, which makes it easy for us to understand and interact with the requests.

const server = http.createServer(app) // here, we initialize a server using our express app.
const gameServer = new Server({ server }) // This line adds Colyseus, the game framework, to our Express server.

app.use('/colyseus', monitor()) // This sets up a route allowing us to view all the Colyseus data in real-time from a browser. We'll use it later.

app.get('/', (req: express.Request, res: express.Response) => {
  res.sendFile(path.resolve('game.html')) // Respond with the game file when the user visits the server. Path.resolve makes sure the path is absolute so Express can find the file.
})

gameServer.listen(port) // Finally, we start the server by listening to incoming requests.
console.log(`Listening on http://localhost:${ port }`)
```

Finally, we need to create the HTML and JS files that will run on the user's computer when they visit the game. Create two new files, `game.html` and `game.js`. You can leave `game.js` empty for now but let's add some basic HTML with a title, page header, and script tag to import `game.js` to `game.html`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Amazing Game</title>
  </head>
  <body>
    <h1>My Amazing Game</h1>
    
    <script src='game.js'></script>
  </body>
</html>
```

Your project should now look like this:

![The beginning of the project](/img/uploads/hcmpg_pbeginning.png "Your project should look something like this")

(it's ok if you don't see `package-lock.json`)

Let's make sure it's working so far - press Run and hold your breath! After several seconds, if you see a new screen open that says "My Amazing Game", it's working!!

![The empty game page](/img/uploads/hcmpg_hello.png "The empty game page")

Yeahhh!!! It works!!! If it doesn't, go back and make sure all your files, including game.html, index.ts, tsconfig.json and package.json match mine.

## Writing the Game

