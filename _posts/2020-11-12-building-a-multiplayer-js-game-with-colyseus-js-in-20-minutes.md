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

Most browser-based games use what are called *websockets* to communicate with a *game server*, a special kind of web server responsible for synchronizing each player's movements across every player's browser. Websockets are not like traditional HTTP requests, as they form a *persistent, 2-way connection* with the server, allowing each server and client to send messages back and forth to each other. For example, if one player in the game moves or does some other game action (depending on the type of game), the game will immediately send that move to the server using a socket connection, where the server will process the move (and perform any validation necessary to prevent cheating), and send it back over a socket connection to all the other players, whose games will process the new data and update the first player's position, so that everyone can see when one player moves. 

Fortunately, the browser's [WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) is easy enough to use already, but for this tutorial we will be using a JavaScript package called [Colyseus](https://colyseus.io/) that handles the websocket connection and game state automatically, so we can just focus on the game itself.

As for the game itself, we will just be using the native browser [canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) as we don't need anything more than that for this simple game. However, if you want to make more complex games in the future there are many available frameworks such as [Phaser.js](https://phaser.io/) that provide useful tools such as physics and animation engines.

We will be using the [TypeScript programming language](https://www.typescriptlang.org/docs/) for the server, as it is the recommended language for Colyseus. If you are not familiar with TypeScript, it is very similar to Node.js but requires you to specify types (such as string, number, or "any") for variables and also lets you use some advanced/modern features that are unavailable in basic Node.js. But usually, you should be able to just write standard JS code and get away with it in TypeScript.

As this is an intermediate-level tutorial, I will assume you are already familiar with JavaScript features such as classes, ternary expressions, and arrow functions.

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

// On REPL, Colyseus doesn't work over HTTPS without additional configuration (or writing custom matchmaking routes). For building this pong workshop this workaround is necesarry to make it work on Repl, but make sure to remove this if you expand your game into a full website with many people playing it, as this effectively disables encryption to prevent mixed content errors.
app.use((req, res, next) => {
  if (req.headers['x-forwarded-proto'] === 'http') {
    next()
  } else {
    return res.redirect(302, 'http://' + req.headers.host + req.url) 
  }
})

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

## Starting the game

Ok, it's time to start coding the game itself!! Let's start by adding the Colyseus library to the `game.html`, before the closing `</head>` tag:

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
const userName = prompt("Choose a name:") || 'player' // Fall back to "player" if the user doesn't enter a name
```

Then, let's get the canvas context:

```javascript
const canvas = document.getElementById('game-canvas')
const width = canvas.width, height = canvas.height
const ctx = canvas.getContext('2d') // This is the canvas context, which we can use to draw things on the canvas.
```

### Making a game loop

Most games have at least one or more *game loop*, that is, a chunk of code composed of an update and drawing function, or maybe a function to calculate physics, etc, that run as frequently as possible while a game is running (this is often 60 times per second). We need one of these when we are using the HTML canvas element as well, because the canvas is quite literally a "canvas" in the sense that you can't adjust the position of something (like a player) or remove something from the canvas (you can only add things) - instead, you have to clear the whole canvas and redraw every object on it, each with their new updated positions. Fortunately, this is easy to in JavaScript.

We will create a function called "loop", which we will pass to a built in browser function called `requestAnimationFrame`. requestAnimationFrame registers a function to be called as soon as possible before the browser repaints the page (which happens roughly 60 times per second on most displays). It's the perfect place to call our game loop to make our game graphics appear as smooth as possible.

It's also important to keep track of when exactly the loop runs every time, so that we can calculate how many milliseconds it's been since the loop last ran. This is a concept known as "delta time" and it's extremely important for ensuring smooth, consistent gameplay regardless of how fast a player's computer is. Fortunately, `requestAnimationFrame` passes the number of milliseconds since the window was loaded into the loop function, which we can use to calculate the delta time.

We'll also add a function called `draw`, where we will eventually place all the code for drawing the rackets and pong ball to the screen.

```javascript
function draw () {
  
}

let lastRender = 0 // Initialize lastRender variable to keep track of when the loop was last run.
function loop(timestamp) {
  var delta = timestamp - lastRender // How many milliseconds have past since the loop last ran?

  // Erase the canvas and refill with black every time the loop runs
  ctx.fillStyle = 'black'
  ctx.clearRect(0, 0, width, height)
  ctx.fillRect(0, 0, width, height)

  draw() // Draw everything

  lastRender = timestamp // Update the last render variable
  window.requestAnimationFrame(loop) // Schedule this function to be run again.
}

window.requestAnimationFrame(loop) // Schedule the loop function to be run next frame
```

Once you add all this, you should be able to refresh the game window and see a black square where the canvas is:

![The blank canvas and script](/img/uploads/hcmpg_blacksquare.png "The blank canvas and script")

### Handling user input

The only user input we need to handle is the browser's left and right arrow keys, which will move the player's racket left or right. We need to be able to check from the game loop to see if the left or right arrow keys are pressed, so let's add some event handlers that will detect when the user presses or releases the left/right arrows and update a variable.

Add two variables to the top of the `game.js` file:

```javascript
let leftIsPressed, rightIsPressed
```

And at the bottom of the file, add a few event handlers to change these variables:

```javascript
window.onkeydown = function (e) {
  if (e.key = 'ArrowLeft') leftIsPressed = true
  if (e.key = 'ArrowRight') rightIsPressed = true
}

window.onkeyup = function (e) {
  if (e.key = 'ArrowLeft') leftIsPressed = false
  if (e.key = 'ArrowRight') rightIsPressed = false
}
```

### Communicating with the server

We need to start a connection with the server before we are able to find or join a pong game. We can do this somewhere near the top of `game.js` by adding this line:

```javascript
// Connect to the colyseus server on port 3000:
const client = new Colyseus.Client(`ws://${window.location.hostname}:3000`)
```

Then, we want to find or create a pong game, which is easy thanks to Colyseus's room system:

```
let room // We'll store the room in this variable
let isPlayer1 // Keep track of who's player 1 and 2
client.joinOrCreate('pong', { name: userName || 'player'})
.then(r => { // We successfully found or created a pong game
  console.log("joined successfully", room)
  room = r
  room.onMessage('youArePlayer1', m => { isPlayer1 = true }) // If the server tells us we're player 1, set isPlayer1 to true
}).catch(e => { // Something went wrong
  console.error("couldn't join room", e)
})
```
Note that this code won't work now, as we haven't yet added the Pong room on the server.

## Coding the server logic

Let's move back to the server now and write the Pong server logic.

First, we need to create a `Room` class (a room is basically an instance of a game) to define the networking code and logic for pong games. We also need to create a `Schema` class to define the "state" for each pong game. The state will include the positions of each player's racket and of the ball, as well as the score, etc. Let's create a new file named `PongRoom.ts` and import some Colyseus classes:

```javascript
import { Room, Client } from "colyseus"
import { Schema, type } from "@colyseus/schema"
```

Now, let's start extending these classes to define a pong room. Each Colyseus room class has several overridable methods, including an `onCreate` method, where we will do some setup:

```javascript
export class PongRoom extends Room {

  onCreate (options: any) {
    this.setState(new PongState()) // Set the state for the room
    this.setSimulationInterval(delta => this.update(delta)) // Set a "simulation interval" aka an update function (similar to the loop function in game.js)
    this.setPatchRate(20) // The patch rate determines the interval (in milliseconds) at which the server sends state updates to the client
    this.maxClients = 2 // Only 2 players per Pong game
    this.clock.start() // Start the game clock
  }

  update (delta: number) {
    
  }

  startGame() {
    
  }

  onJoin (client: Client, options: any) {

  }

  onLeave (client: Client, consented: boolean) {

  }
}

```

### Adding game state

Let's add some classes and variables to keep track of the game's state. **Make sure you add these above the `PongRoom class** First, add a "Player" class to keep track of each player's racket position, score, whether they've won, and name:

```javascript
class Player extends Schema {
  // This class keeps track of the racket position, score and name for a player
  @type('number')
  racketX: number = 250 // Initializing it at 270 will make sure it's centered

  @type('int8')
  score: number = 0

  @type('boolean')
  hasWon: boolean = false

  @type('string')
  name: string

  @type('string')
  clientId: string // We'll use this to keep track of which player is which
}
```

Note that the `@type...` part is a typescript feature called a _decorator_. In this context, it's used to tell Colyseus what type of data a variable is so that it can properly encode it for websocket transfer.

Next, we'll add a PongState class to store the players and whether the game has started, as well as the position and direction of the Pong:

```javascript
class PongState extends Schema {
  // Has the game started?
  @type('boolean')
  gameStarted: boolean = false

  // We instantiate two player classes, one for each player
  @type(Player)
  player1: Player = new Player() 
  @type(Player)
  player2: Player = new Player()

  // We also define a few variables to keep track of the Pong
  @type('number')
  pongX: number
  @type('number')
  pongY: number
  @type('boolean')
  pongDirection: boolean // 1 means it's flying towards player 2, 0 means it's flying toward player 1
  @type('float32')
  pongAngle: number // 0 means it's flying in a straight line, 1 is 45 degrees right, -1 is 45 degrees left
}
```

### Handling when a user joins

Back in the `onJoin` function of `PongRoom`, we need to do two things when a user joins. First, we need to add the user's name and id to the game state, then call the `startGame` function if there are 2 players:
 
```javascript
  onJoin (client: Client, options: any) {
    // Determine whether this is player 2 or player 1 joining. If player 1 already exists then this is player 2.
    const alreadyHasPlayer1 = !!this.state.player1.clientId
    const newPlayerState = alreadyHasPlayer1 ? this.state.player2 : this.state.player1

    // Set the player's name and ID:

    newPlayerState.name = options.name.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;") // options contain options passed from the player. We'll write that part soon. The 3 replace statements just make sure the user's name doesn't contain any dangerous characters.

    newPlayerState.clientId = client.sessionId

    if (alreadyHasPlayer1) {
      // We now have 2 players and can start the game!!!
      this.clock.setTimeout(() => this.startGame(), 2000) // Wait 2 seconds before starting
    } else {
      client.send('youArePlayer1') // This is player 1, make sure to let them know!
    }
  }
```

### Starting the game

Now, in the `startGame` function, all we have to do is choose a random position for the pong, reset it and start the game:

```javascript
  startGame() {
    this.state.pongDirection = Math.random() <= 0.5 // Randomize the starting direction of the pong
    // Reset the position and angle
    this.state.pongX = 300
    this.state.pongY = 300 
    this.state.pongAngle = 0
    this.state.gameStarted = true // Start the game!!!
  }
```

### Handling when a user moves their racket

Now, we need to add a websocket message handler so that each client can tell the server when the player moves their racket (which we'll code soon). When they do, we need to update the racket state. We can use the `room.onMessage` handler for this. Let's add it to the bottom of the `PongRoom`'s `onCreate` method.

```javascript
  onCreate (options: any) {
    this.setState(new PongState()) // Set the state for the room
    this.setSimulationInterval(delta => this.update(delta)) // Set a "simulation interval" aka an update function (similar to the loop function in game.js)
    this.maxClients = 2 // Only 2 players per Pong game

    this.onMessage('moveRacket', (client, data) => {
      // First, we check the client's id to see whether they're player 1 or player 2
      const player = (client.sessionId === this.state.player1.clientId) ? this.state.player1 : this.state.player2

      player.racketX += data.move // Adjust the player's pong position. data is passed from the player; we'll code that soon.

      player.racketX = Math.min(Math.max(player.racketX, 0), 500) // We clamp the paddle position so the player can't move it off the canvas
    })
  }
```

### Adding the PongRoom to the server

Now that we've written all the game logic for our pong room, we actually need to tell the server about it. Let's import the room code at the top of `index.ts`, right below all the other `import` statements:

```
import PongRoom from "./PongRoom"
```

and define the room for the server, *above* the line that says `app.use('/colyseus', monitor())`:

```
gameServer.define('pong', PongRoom) // Add the pong room to the server
```

The server is almost finished!

## Coding the game

Now it's time to finish writing the game logic and drawing functions that actually draw the rackets and ball to the canvas.

### The status text

We need some sort of status text to tell the player whether the game has started and who they're playing against, etc. We could add this as text to the canvas, but I prefer to add UI elements like status text as HTML around the canvas, as it's easier to program and style. So let's add a new `<p>` tag to `game.html`, right between the header and canvas. By default, it will say "Waiting for opponent". Let's also update the title:

```html
  <body>
    <h1>Pong</h1>
    <p id='game-status-text'>Waiting for opponent...</p>
    <canvas id='game-canvas' width='600' height='600'></canvas>
    
    <script src='/game.js'></script>
  </body>
```

Now, let's create a reference to this element at the top of `game.js`:

```js
const gameStatusText = document.getElementById('game-status-text')
```

In the loop function, let's add a `if/else` block around the `draw` function to make sure it doesn't run unless the game has started, and let's also update the status text when a game starts:

```javascript
if (room && room.state.gameStarted) { // Don't draw anything until the game has started
    gameStatusText.innerText = `${room.state.player1.name} vs ${room.state.player2.name}`
    draw() // Draw everything
  }
```

### The rackets

We can easily draw the rackets by getting their positions from `room.state`, which is always up-to-date with the latest state on the server. Add functions to draw the two rectangles to the `draw` function in `game.js`:

```javascript
function draw () {
  if (!room || !room.state.gameStarted) return // Don't draw anything until the game has started
  // Draw the rackets
  ctx.fillStyle = 'white' // set the color
  // Draw the bottom racket with a width of 100 and height of 20 (bottom racket is always this player's racket)
  ctx.fillRect(isPlayer1 ? room.state.player1.racketX : room.state.player2.pongX, height - 20, 100, 20)
  // Draw opponent's top racket
  ctx.fillRect(isPlayer1 ? room.state.player2.racketX : room.state.player1.racketX, 0, 100, 20)
}
```

### A brief exploration of game networking techniques

Before we write the code to update the racket positions, I should briefly talk about multiplayer game networking architecture. What we're about to do here - that is, take user input and use it to calculate an incremental movement of the racket each frame, then send that incremental movement to the server to add to the current position - would not usually be the optimal way to handle movement networking, _on its own_. There are many different game networking architectures, each with their own benefits and caveats. In most games, the two enemies of game networking are _networking latency_ (lag) and _cheaters_. Nowadays, to solve the latter problem, most games use some type of _authoritative server_ design where the server controls and determines every action that happens in a game and is the sole source of truth for clients. Rather than relying on game clients to run calculations, for example, the client might just tell the server what direction the player is moving in, and the server does the calculations and sends the results to the other clients. This precludes the possibility of hackers tampering with this logic in their own game clients.

However, authoritative server design causes serious latency problems because it can take quite a while (relatively speaking) for one user's movements to reach the other players. This can lead to a bad user experience for players. Usually, the solution to this is quite complex, involving predicting the user's movements on the server or client if possible, then reconciling changes on the client or server, or interpolating objects between positions and constructing a timeline for each client on the server with lag compensation and using that to figure out the authoritative order of events, and more, or just some of the above or a combination of the above.

These are all things to keep in mind, but obviously for a simple, casual, slow-paced game like Pong we don't really have to worry about any of it. Sending user input to the server and just updating the player's game from the server every frame is good as it is mostly authoritative (although note that in this tutorial, **we won't be doing any validation on the user input** so cheating is still easy), and luckily the slight amount of lag it may cause actually contributes to an old-fashioned effect and feel for the controls. Anyway, I've already spent longer than I intended to talking about networking architecture, so let's get on with the code! If you're interesting in exploring game networking further, I recommend starting with [this series on server-client architecture](https://www.gabrielgambetta.com/client-server-game-architecture.html).

### Updating the racket positions

After all that, it turns out our method's super easy to add to the code (just 2 lines!). For our purposes, we can simply assume that the user has been holding the left or right key down since the last loop ran, and we can use delta time (the time since the last time the loop was called) to calculate how many pixels the racket should move. So, if the player is holding one of the keys down when the loop runs, we can take deltatime and divide it by 2 to make racket movement a little slower (so, holding the left key down for one second = 1000ms/2 = 500 = the racket moves 500 pixels to the left) and send that value to the server (where it will be handled by the function we wrote earlier).

Let's add this in the `loop` function of `game.js` right before the `if` statement:

```javascript
  if (leftIsPressed) room.send('moveRacket', { move: -(delta / 2) }) // Negative sign so the racket moves left
  if (rightIsPressed) room.send('moveRacket', { move: (delta / 2) })
```

That's it! If you press run to restart the server and then open your Repl in two different tabs, you should be able to move a racket in one tab with the left/right arrows and see it moving in both tabs!

### The pong ball

Now we show movement of the pong ball on the canvas. It will be simulated on the server, so all we need to do is draw it to the screen. Let's add it the end of the `draw` function in `game.js`:

```javascript
  // Draw the pong ball
  ctx.beginPath() // Start a new drawing path
  ctx.arc(room.state.pongX, room.state.pongY, 10, 0, 2 * Math.PI) // Draw the ball with a radius of 20
  ctx.fill() // fill it in
```

### The score

Let's add two numbers to the canvas as well, to display the score (which does not yet change, but it will soon!). Add this code to the bottom of the `draw` function:

```javascript
  // Draw the score
  ctx.font = '30px Arial'
  ctx.fillText(isPlayer1 ? room.state.player1.score : room.state.player2.score, 15,  height - 60) // The bottom player's score
  ctx.fillText(isPlayer1 ? room.state.player2.score : room.state.player1.score, 15, 45) // The top player's score
```

## Simulating pong ball movement on the server

Open `PongRoom.ts` again and scroll down to the `update` function in the `PongRoom` class. You may remember we set this up earlier as a "simulation interval". It runs 60 frames per second by default and we'll use it to update the position of the Pong ball each frame.

Here's how it will work:

1. We keep track of the x position, y position (where a greater y value is closer to player 2), direction (1 means it's flying toward player 2, 0 means it's flying toward player 1) and angle (-1 means it's aiming 45 degrees left, 1 means it's aiming 45 degrees right, 0 means it's flying straight up or down) of the pong ball in the room state (we added this earlier).
2. Each update, we move it (delta/3) y pixels in whichever direction it's travelling. This means it would move 1000 pixels across the canvas in 3 seconds.. We add (the angle value * (delta/3)) to the current x value to get a new x value for the ball.
3. If we detect the ball has moved within the "goal" area of either player (within 20px of the end), we check to see if it collided with the racket.
4. If it **did not** collide, we reset the ball and increment the other player's score.
5. If it **did** collide, we switch the direction of the ball, and calculate the new direction it's flying in using this formula: (ball x position - center of racket x position) / half of racket width (which is 50). Note that you can adjust this formula if you want the ball to bounce differently. 50 for the last variable would give a maximum bounce angle of 45 degrees (assuming the racket is 100px wide), but you can decrease that number if you want a greater bounce angle.
6. If we detect the ball touching a side of the canvas, we flip the angle so it bounces back.
7. If either player has more than 10 points, set `hasWon` to true for that player and destroy/disconnect the room.

Here's the final function:

```javascript
  update (delta: number) {
    if (!this.state.gameStarted) return // Don't update if the game hasn't started

    const speedConstant = delta / 3

    // Update the ball's position:
    if (this.state.pongDirection == 1) this.state.pongY += speedConstant // Ball is moving TOWARD player 2
    else this.state.pongY -= speedConstant // ball is moving away from player 2

    this.state.pongX += (speedConstant * this.state.pongAngle) // Change the x value depending on the angle

    if (this.state.pongY + 10 >= 580 || this.state.pongY - 10 <= 20) // If ball is touching goal zone (+- 10 to account for radius)...
    {
      const isOnPlayer1Side = this.state.pongY - 10 <= 20 // Is it on player 1's side or player 2's?

      const racketX = isOnPlayer1Side ? this.state.player1.racketX : this.state.player2.racketX

      if (this.state.pongX >= racketX && this.state.pongX <= racketX + 100 ) { // Ball collided with racket!!!
        this.state.pongDirection = !this.state.pongDirection // Flip the direction
        this.state.pongAngle = (this.state.pongX - (racketX + 50)) / 50 // Calculate the new angle between -1 and 1
        this.state.pongY = isOnPlayer1Side ? 30 : 570 // Move the ball to the edge of the racket to make sure it doesn't get stuck
      } else { // Ball did not collide with racket - SCORE!!!
        isOnPlayer1Side ? this.state.player2.score += 1 : this.state.player1.score +=1 // Increment the other player's score
        
        this.startGame() // We can just reuse the startgame function to start a new round
      }
    } else if (this.state.pongX > 590 || this.state.pongX < 10) { // Ball is touching edge of canvas!
      this.state.pongAngle *= -1 // Flip the angle so the ball bounces back
    }

    if (this.state.player1.score >= 10 || this.state.player2.score >= 10) { // One of the players has won
      const playerThatWon = this.state.player1.score >= 10 ? this.state.player1 : this.state.player2

      playerThatWon.hasWon = true

      this.broadcastPatch() // Broadcast the new state to make sure each player knows who won before we disconnect them

      this.disconnect() // Disconnect players from room and dispose
    }
  }
```

But this won't work properly in the game, because the pong Y value represents how close the ball is to player 2. So because each player has their own racket on the bottom of the canvas, we have to flip this y coordinate for player 1 in `game.js`. Replace the line that says `ctx.arc` in the `draw` function with this:

```javascript
  const pongY = isPlayer1 ? 600 - room.state.pongY : room.state.pongY // For player 1 we should flip the direction of the ball
  ctx.arc(room.state.pongX, pongY, 10, 0, 2 * Math.PI) // Draw the ball with a radius of 20
```

## Final touches

The game is almost, done but we have a few last things to do. First, we need to tell the player that the game is over when the server disconnects. Then, we need to check to see if either player won and display that as well. We'll just do this in the status text.

However, you may remember we added an `if` block to the loop in `game.js` earlier that rewrites the status text every loop when the game starts So, let's change that, and rather than setting the status text every loop let's just change it once when the game begins. We can do this using the Colyseus `state.listen` function. First, delete the line in `loop` that begins with `gameStatusText.innerText =` and then let's add the game start listener to the bottom of the `.then()` callback near the top of the file:

```javascript
  room.state.listen('gameStarted', (currentValue, oldValue) => {
    // If the game has started and it wasn't started previously, update the status text
    if (currentValue && !oldValue) gameStatusText.innerText = `${room.state.player1.name} vs ${room.state.player2.name}`
  })

```
Now, we can add another listener in the same place (right below the gameStarted listener) that listens for the server to disconnect:

```javascript
    room.onLeave((code) => {
    gameStatusText.innerText = 'Game Over. ' // We were disconnected from the game (either intentionally or because of an error), let the player know it's game over.
    // Let the user know if either player won:
    if (room.state.player1.hasWon) gameStatusText.innerText +=  `${room.state.player1.name} won!!!`
    else if (room.state.player2.hasWon) gameStatusText.innerText += ` ${room.state.player2.name} won!!!`
    else gameStatusText.innerText += ' A player disconnected.' // if neither player won, that means one of the players disconnected before the game was finished.
    gameStatusText.innerText += ' Reload the page to find a new game.' // Tell the player how they can find a new game.
  })
```

But we don't actually _do_ anything on the server when one player disconnects, so if a player disconnects the other player will just be left stranded with a broken game. We can solve this on the server, in `PongRoom.ts`, by disconnecting ALL players from the game as soon as one leaves. We'll add this to the `PongRoom` class's `onLeave` method, which runs whenever a player disconnects from the room:

```javascript
  onLeave (client: Client, consented: boolean) {
    this.disconnect() // If a player leaves the game is unplayable, so destroy the room and disconnect the remaining player so that they can find a new game.
  }
```

### Adding a short delay between rounds

We should also add a short delay between rounds, to give the players time to readjust their rackets. To do this, let's add a new property to the `PongRoom` class (right at the top before any methods):

```
export default class PongRoom extends Room {
  roundIsRunning: boolean
```

Then somewhere in `startGame`, we can add:

```
this.roundIsRunning = true
```

And, in `update`, we can replace the line that starts a new round (`this.startGame()`) with:

```
        this.roundIsRunning = false
        this.clock.setTimeout(() => this.startGame(), 1000) // Wait 1 second before starting next round
```
And finally, we can change the top line in `update` to:

```javascript
if (!this.state.gameStarted || !this.roundIsRunning) return // Don't update if the game or round hasn't started
```
so that it doesn't update unless a round is running.

### Making it slightly more difficult

There are many ways once could improve this game by making it look better, perform better with less lag, or making it more difficult. Here, we will add one such addition to demonstrate how you can add new features. 

In most pong games, the ball speed starts out slow and gradually increases each match. Let's add that to this game. 

First, we need to add a new state variable to keep track of when each match starts. let's add that to the top of the `PongState` class in `PongRoom.ts`:

```javascript
  @type('number')
  roundStartedAt: number
```

Then, let's set that number every time a new game starts somewhere in `startGame`:

```
this.state.roundStartedAt = this.clock.elapsedTime // Set the round started time using the timestamp from the game clock
```
Now, let's double the speed of the pong every thirty seconds from the `update` function. To do that, we'll subtract the elapsed time (ms) at the start of the round from the current elapsed time (ms), and divide it by 30,000 to see how many times 30 seconds have passed (we'll also add 1 so that it starts at 1 rather than 0) We'll multiply this by our speed constant in the `update` function, like this:

```javascript
const timeMsSinceRoundStart = this.clock.elapsedTime - this.state.roundStartedAt
    const speedConstant = (delta / 3) * (timeMsSinceRoundStart / 30000 + 1) // Calculate the speed constant for the ball. it should gradually increase over time.
```

And, that's all for this pong game today!!

## Conclusion