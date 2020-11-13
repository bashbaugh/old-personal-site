---
layout: post
section-type: post
date: 2020-11-12
lastmod: 2020-11-12
title: Building a Multiplayer JS Pong Game with Colyseus.js in 20 Minutes
slug: multiplayer-js-game-workshop
url_slug: multiplayer-js-game-workshop
category: tutorials
tags:
  - code
  - javascript
  - game
  - tutorial
postdescription: Using TypeScript along with Colyseus.js we will create a very
  simple multiplayer browser pong game in less than 30 minutes. Originally
  uploaded as a Hack Club Workshop.
titletag: Building a multiplayer JS Pong game with Colyseus.js in 20 minutes
description: Using TypeScript along with Colyseus.js we will create a very
  simple multiplayer browser pong game in less than 30 minutes. Originally
  uploaded as a Hack Club Workshop.
---
In this tutorial, I will explain the basics of creating a multiplayer browser pong game with the JS framework [Colyseus.js](https://docs.colyseus.io/).

## How it works

Most browser-based games use what are called _websockets_ to communicate with a _game server_, a special kind of web server responsible for synchronizing each player's movements across every player's browser. Websockets are not like traditional HTTP requests, as they form a _persistent, 2-way connection_ with the server, allowing each server and client to send messages back and forth to each other. For example, if one player in the game moves or does some other game action (depending on the type of game), the game will immediately send that move to the server using a socket connection, where the server will process the move (and perform any validation necessary to prevent cheating), and send it back over a socket connection to all the other players, whose games will process the new data and update the first player's position, so that everyone can see when one player moves. 

Fortunately, the browser's [WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) is easy enough to use already, but for this tutorial we will be using a JavaScript package called [Colyseus](https://colyseus.io/) that handles the websocket connection and game state automatically, so we can just focus on the game itself.

As for the game itself, we will just be using the native browser [canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) as we don't need anything more than that for this simple game. However, if you want to make more complex games in the future there are many available frameworks such as [Phaser.js](https://phaser.io/) that provide useful tools such as physics and animation engines.

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

app.get('/game.js', (req: express.Request, res: express.Response) => {
  res.sendFile(path.resolve('game.js')) // Send game.js as well. (we'll create these files soon)
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

Ok, it's time to code the game itself!! Let's start by adding the Colyseus library to the `game.html`, before the closing `</head>` tag:

```html
  <head>
    <title>My Amazing Game</title>
    <script src="https://unpkg.com/colyseus.js@^0.14.0/dist/colyseus.js"></script>
  </head>
```

Now let's add a `canvas` element for our game graphics to the `<body>`:

```html
  <body>
    <h1>My Amazing Game</h1>

    <canvas id='game-canvas' width='400' height='600'></canvas>
    
    <script src='game.js'></script>
  </body>
```

Now, in `game.js` the first thing we want to do is ask the player to enter a display name to identify themself to other players.

```javascript
const userName = prompt("Enter a name:")
```

Then, let's get the canvas context:

```javascript
const canvas = document.getElementById('game-canvas')
const width = canvas.width, height = canvas.height
const ctx = canvas.getContext('2d') // This is the canvas context, which we can use to draw things on the canvas.
```

### Making a game loop

Most games have at least one or more _game loop_, that is, a chunk of code composed of an update and drawing function, or maybe a function to calculate physics, etc, that run as frequently as possible while a game is running (this is often 60 times per second). We need one of these when we are using the HTML canvas element as well, because the canvas is quite literally a "canvas" in the sense that you can't adjust the position of something (like a player) or remove something from the canvas (you can only add things) - instead, you have to clear the whole canvas and redraw every object on it, each with their new updated positions. Fortunately, this is easy to in JavaScript.

We will create a function called "loop", which we will pass to a built in browser function called `requestAnimationFrame`. requestAnimationFrame registers a function to be called as soon as possible before the browser repaints the page (which happens roughly 60 times per second on most displays). It's the perfect place to call our game loop to make our game graphics appear as smooth as possible.

It's also important to keep track of when exactly the loop runs every time, so that we can calculate how many milliseconds it's been since the loop last ran. This is a concept known as "delta time" and it's extremely important for ensuring smooth, consistent gameplay regardless of how fast a player's computer is. Fortunately, `requestAnimationFrame` passes the number of milliseconds since the window was loaded into the loop function, which we can use to calculate the delta time.

```javascript
let lastRender = 0 // Initialize lastRender variable to keep track of when the loop was last run.
function loop(timestamp) {
  var delta = timestamp - lastRender // How many milliseconds have past since the loop last ran?

  // Erase the canvas every time the loop runs
  ctx.fillStyle = 'black'
  ctx.fillRect(0, 0, width, height)


  lastRender = timestamp // Update the last render variable
  window.requestAnimationFrame(loop) // Schedule this function to be run again.
}

window.requestAnimationFrame(loop) // Schedule the loop function to be run next frame
```



