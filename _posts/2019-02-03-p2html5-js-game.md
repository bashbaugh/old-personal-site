---
layout: post
section-type: post

date: 2019-02-03
#update-text: 
#lastmod: yyyy-mm-dd

title: "Make a fun HTML & JavaScript game - part 2: JavaScript tutorial"
image: "/img/post/js-game/game.jpeg"
top-image: "/img/post/js-game/js-html-game.jpeg"
slug: "html-js-game-part2"
category: "code"
tags: [ 'web-dev', 'game', 'code', 'project', 'tutorial' ]

postdescription: "Learn how to create a simple but fun online browser-based game with HTML and JavaScript. Part 2: JavaScript tutorial - learn how to add interactivity to web pages using JS."

titletag: "Make a fun HTML & JavaScript game - part 2: JavaScript tutorial"
description: "Learn how to create a simple but fun online browser-based game with HTML and JavaScript. Part 2: JavaScript tutorial - learn how to add interactivity to web pages using JS."
---

<script src="/js/jscolor.js"></script>

![banner image](/img/post/js-game/js-html-game.jpeg "js + html")

In this tutorial series, I will guide you through the steps to create your own simple but fun online browser-based game, using HTML and JavaScript. 

This is part two of the tutorial. **If you have not read part one, you should read it now.** [Click here for part 1: HTML tutorial](/code/2019/html-js-game-part1). Once you have read it, you can continue with this part.

In this part of the tutorial, I will show you how to add JavaScript to program the game itself. Keep in mind that this tutorial is meant for beginners (or almost-beginners), but I will only cover what is needed to teach you how to make this game. It may seem to complicated or hard at first, so if it does check out some of the links at the bottom and come back when you feel a little more comfortable with JavaScript basics. 

**I will include links at the bottom of this article for reference for every function we use.**

Here's a reminder of what we did in [the last part](/code/2019/html-js-game-part1):

+ We learned the basics of HTML, which uses `<tags>` to tell the browser more about different elements on the page, for example the `<i></i>` tags that _italicize_ stuff.
+ We learned how JavaScript can be added to websites to make them interactive or add other programmed functionality.
+ We learned how to get and use a _text editor_, or code editor, which makes code look better. If you haven't got one yet, now might be a good time to get one. (Or else you could [use the online TryIt editor](https://www.w3schools.com/code/tryit.asp?filename=FZYQJ01WUF5V))
+ And finally we created a simple web page for the game to live in. We used a `<canvas>` element to create a "canvas" for the game.

Here's the code for the web page so far:

<script src="https://gist.github.com/scitronboy/d5f5017fadffcdb0dc3906cef590a45d.js"></script>

Let's start! 

### JavaScript Basics

JavaScript is put between `<script>` tags in a web page. Then, when a browser is loading a page, it reads the HTML line by line, and when it comes to a `<script>` tag, it knows everything from there until the closing `</script>` tag is JavaScript. So, it reads the JavaScript line by line and follows the instructions on each one. Here I will cover a few of the different things you can do on each line in JavaScript.

#### The semicolon

In JavaScript you should put a semicolon after every command, to tell the browser that that command is finished. This is not strictly required, but it is recommended. Here's an example:

`command;`

`do something else;`

Keep in mind that there is usually one command per line.

#### Variables

Variables are a crucial part of most programming languages. They are simply a place where a program (the code) can store information, such as words, or numbers (think score, current location, whether or not a user has clicked a certain button, etc.)

Each variable in JavaScript has a name and a value. Think of them like boxes - each variable is a box that has a label written on the outside, and the computer can store numbers or other information inside the box. Then whenever it needs the information again it can look in the box. It can also change what's in the box, if it needs to.

In JavaScript you declare (create) a variable by using an equals sign with the `let` keyword. (Please note: you may see other people use the  `var` keyword in JavaScript. Know that `var` can also be used to create a variable, however it is generally better to use the `let` keyword for reasons I will not explain here. If you would like to know why, please leave a comment at the end of this article.) Here's an example:

`let score = 50;`

In that example, we are creating a variable (like a box) named `score`, and setting it equal to `50`. The semicolon signifies the end of the command. Then if the player got 20 more points, we could change the variable to 70. Note that, after the variable is created, we do not need to use `let` anymore:

`score = 70;`

Or else we could just set `score` to itself + 20:

`score = score + 20;`

Since `score` _was_ 50, and 50 + 20 = 70, `score` now equals 70.

In JavaScript you can also have word variables, but you **_MUST_** put the word between two double quotes (""). Words, sentences, and letters are called "strings" in programming (think of a "string" of letters):

`let my_first_name = "John";`

You can see in the last example that I didn't name the variable `my first name`, I named it `my_first_name` with an underscore instead of a space. This is very important because **variable names cannot have spaces**.

You can also do "addition" with strings. Here's an example:

`let my_last_name = "Smith";`

`let my_full_name = my_first_name + " " + my_last_name`

Now `my_full_name` is equal to `John Smith` because we created it from my_first_name (which was "John") plus a space (" ") plus my_last_name (which was "Smith").

#### Functions

Functions are like commands that tell the browser to do something. There are many built in functions, and you can also create your own. In JavaScript, you can call (execute) a function by simply typing the command followed by two parenthesis. The parenthesis are used to pass (give) parameters, or arguments, for the function to use. Arguments are put between the parenthesis, with a comma separating each one, like this:

`function(argument_1, argument_2, argument_3);`

For example, imagine a scenario where you have a function named `eat` that tells the computer to eat (yes, yes, I know computers can't eat). But what if the computer can eat different things? How does it know what to eat? You could create different functions for different foods, like `eat_orange` and `eat_apple`, but that gets clunky and is _not_ a good solution, because you'd have to write a new function every time you added a food item. The solution is to pass the `eat` function an argument every time that tells it _what_ to eat. An example:

`eat("apple");` or `eat("orange");` 

Then when the function receives the argument it processes it and figures out what to eat.

##### The `console.log` command

The console.log command is very useful for programming JavaScript. It can be used to send a message to the console, which is something that is integrated into most browsers that allows developers to interact with the JavaScript on a page. Try it out! Open up your `game.html` file and add this command in between the `<script>` tags:

`console.log("Hello, Console! This is JavaScript!")`

Load the page in your browser. Then open up your console. Usually, you can do this by pressing `Ctrl+Shift+I` or else right clicking the page and pressing "Inspect". Then, click on "console" when the window opens. Here is an example on Chrome:

![image of console](/img/post/js-game/console-hello.png "image of console")

You can see the message at the top! That was generated by the little line of JavaScript we wrote!

**Note**: the `log` command is a function, however the `console` part is not actually part of the function name. It is instead a "class" that houses the `log` function. I will not cover classes in this tutorial, but just know that function names cannot actually have periods in them. Remember this when you start to create your own functions.

##### Creating your own function

In JavaScript, custom functions are blocks of code that are run when you call the function. They are often used when the same code needs to be run in two different places, because you can just call the function in each place instead of duplicating code. Here's how you create your own function:

`function my_own_function(my_own_argument) {do stuff using my_own_argument;}`

You declare the function using the `function` keyword. Then, you name the function and add any arguments it will require to the parenthesis. Then, you put all the code that will run when you call the function inside curly braces (`{}`).

Here's an example of a function to print a full name to the console:

<script src="https://gist.github.com/scitronboy/b991a6504fad0fa7428dfef266067607.js"></script>

Then let's say we have three names to print. Luckily we can use the name-printer function we just made to help us do that:

<script src="https://gist.github.com/scitronboy/55253735ed5071c86910415435449681.js"></script>

Here is the output (on the console):

    Here is a full name:
    Bob Joe
    Here is a full name:
    Margaret Joe
    Here is a full name:
    Jane Smith
    
Do you get it? We called `print_full_name` three times in that last script, and each time it printed the "Here is a full name" message _and_ the full name which it pieced together using the arguments we passed it.

Don't worry if this is a bit intimidating or confusing. It will probably become clearer later on. I also include links at the bottom to tutorials that you can use to learn more about JavaScript.


### Game Overview

Before you start a project, it's always a good idea to lay out a plan and figure out how you'll do things before you start. So here's how we want the finished game to behave:

+ When you press the up arrow key, the game will start.
+ The player will be a yellow (or any other color) circle
+ There will be colored dots spread throughout the game
+ The colored dots will move down the screen at a fixed rate so that it looks as if the player is moving forward
+ The left and right arrow keys will move the player left and right
+ The goal will be for the player to "fly" over as many colored dots as possible within the time limit. Each time the player flies over a dot, his score will increment by 2.

And here's how we will build it:

1. We will use the canvas to create the game graphics.
2. We will create an `init` function that will run when the page loads, that will add a message to the canvas that says "Press Up To Start"
3. We will detect key presses. The up arrow will trigger the start of the game, and the left and right arrow keys will trigger functions called `move_left` and `move_right` which will move the player to the left and right.
4. We will create a game loop function that updates the game.
5. The game loop function will move the colored dots down slightly each time it is run.
5. It will also have a system to detect whether the player is over a colored dot, so that it can increment the score.
6. Finally, it will erase the canvas, then draw the background, player, and score onto their new positions.
7. We will make the game loop function run many times per second so that the game runs smoothly.


### Programming The Game

Now we will start programming the game. 

Leave the console in you browser open the whole time, because it will alert you of any errors or problems in your code.

_You will get an error near the beginning that says "init is not defined". Just ignore it - we will fix it later._

#### Getting the canvas context

Let's add a little message at the top of the `<script>` section in `game.html` so that we see in the console when the script starts:

`console.log("Starting the script!");`

Now the first thing that we need to do is to get a reference to the canvas on the web page, so that we can "draw" on it with JavaScript. Remember when we created the web page and we added a HTML attribute to the canvas that gave it an id? Specifically, `id="c"`. Now we are going to use that id we added to tell JavaScript what we want. Add this to the top of the `<script>` section:

`let canvas_element = document.getElementById("c");`

`document` is a class that contains a ton of functions for accessing or manipulating the "document", or the web page itself. `getElementById` is a function that gets a reference to an element on the page using its id. So when we call `document.getElementById("c")` JavaScript stores a reference to the element that has an id of `c`, which is our canvas.

Now we need to actually get the "context", or the part of the canvas that will allow us to draw on it. To do this, we can use the `getContext` function of the canvas element:

`let canvas = canvas_element.getContext("2d");`

The argument `2d` just tells `getContext` that we want the 2-dimensional part of the canvas.

<!--Here is what your `game.html` should look like now:

<script src="https://gist.github.com/scitronboy/035e5fb8c4e80299f6de16a96c955569.js"></script>-->

##### A note about the coordinate system

When working with canvases, simple x and y coordinates are used. x is horizontal and is the first argument in functions, and y is vertical and is the second argument in functions. Each unit is 1 pixel. Remember that the _upper left corner_ is origin, so its coordinates are `0,0`. 100 pixels to the right of the upper left would be `100,0`. 100 pixels below the upper left corner is `0,100`. Because our canvas has a width of 400 pixels and a height of 400 pixels, the bottom right corner would be `400,400`.

#### Creating the `draw_background` function

We are going to need this function soon. It will draw a solid color onto the canvas.

The `draw_background` function will have three lines:

1. Clear the canvas. Erase everything in a rectangle from 0,0 to 400,400 (or canvas width, canvas height).
2. Set the color of the "paintbrush". Colors in JavaScript are set using hexadecimal color codes, which store a color's RGB (Red, Green, Blue) values in a shortened format. All color codes must have a `#` in front of them. I chose a greenish color (which has a hex code of  `#1EBA00`) for the background. Here is a color picker that you can use if you want to change the color: <input class="jscolor" value="1EBA00">.
3. Draw a rectangle that covers the entire canvas. 

Here it is. Add this function to the script:

<script src="https://gist.github.com/scitronboy/efeb8dca816d47c0bbe8ebb3fa531455.js"></script>

You can see that `canvas.clearRect` and `canvas.fillRect` have four arguments. They are (upper left corner of rectangle x position, upper left corner of rectangle y position, width of rectangle, height of rectangle).

#### Creating the `init` function

When we created the web page for the game, we added `onload="init()"` as an attributes to the `<body>`. If you have a JavaScript function that needs to be run as soon as an element on the page loads, you can use the `onload` attribute. So `onload="init()"` tells the browser to run (call) the JavaScript function `init()` as soon as the body is loaded. Here it is. All it does is draw a background and message onto the canvas:

<script src="https://gist.github.com/scitronboy/df88ad9a4cba1193da8d0957abdf39f8.js"></script>

Again, feel free to change the color code `#FFFFFF` to something else if you want to: <input class="jscolor" value="FFFFFF">.

Also, it's probably self-evident but the basic arguments for fillText are (text, left side of text x position, left side of text y position).

Make sure you've added all these functions and variables to your project file (`game.html`). It should look like this now (with or without the comments):

<script src="https://gist.github.com/scitronboy/6a84f4306b83f36d751dbe6e6d6e3066.js"></script>

And after you load the file into your web browser and open your console, you should see this:

![The game](/img/post/js-game/game-after-init-func.jpeg "the game")


Congratulations! You may have just written your first piece of JavaScript that does something visible on a web page!

#### Detect key presses

We need to detect key presses and take action when someone presses the up arrow, left arrow, or right arrow.

To do this, we simply "attach" a `key_pressed` function to the documents `onkeydown` attribute, so that when a key is pressed down, `key_pressed` is called. Add this to the script:

`document.onkeydown = key_pressed;`


##### JavaScript lesson: `IF` statements

If statements are used when a program needs to do something ONLY if a condition is true. If the condition is false, then the action is not done. The basic syntax (layout) for an `if` statement in JavaScript is:

`if (this is true) {do everything in here}`

You can optionally extend it and give it something to do IF and ONLY IF the condition is false:

`if (this is true) {do this} else {if its not, do this}`

You can put anything inside the parenthesis as long as it can be true or false. Everything with a value is considered true in JavaScript, so `43` and `1` is considered "true" by JavaScript. `0` is considered false. 

Often you'll need to compare to values in JavaScript, to see if, for example, one value is greater than, less than, or equal to another. In that case you'll need to use [comparison operators](https://www.w3schools.com/js/js_comparisons.asp); some examples are `==` (equal to), `<` (less than), `>` (greater than), and `!=` (not equal to). There are also [logical operators](https://www.w3schools.com/js/js_comparisons.asp) which are `&&` (and), `||` (or), and `!` (not).

Here is an example in JavaScript, given three variables named `happy`,  `sad`, and `name`:

<script src="https://gist.github.com/scitronboy/93b3d8a95ff86d1235fe7a4a1e01391b.js"></script>

Be very careful not to confuse `=` with `==`. If you use an assignment operator (`=`) where you meant to use a comparison operator (`==`) it will result in strange unexpected results.

#### Create the `key_pressed` function

When a key is pressed, `key_pressed` will be called with an argument that contains more information about the key press. That argument will hava a property (variable) named `key` that will contain the name of the key.

So we need to check what key it is and then take action based on that. But how can we check which key it is? We need an `if` statement!

Here's the function:

<script src="https://gist.github.com/scitronboy/e9e8bf2a92a998f3b25eaf212bbf12d8.js"></script>

For now, every time you press one of the arrows it will just log it on the console. Later, we will make the key presses actually do stuff. 

Try it out! Reload the page in your browser, open the console, click on the green canvas, and then try pressing the arrow keys. You should see the console logging them.

Just in case you got lost, [here's what your project file should look like now](https://gist.github.com/scitronboy/ccfddf2989b06cf2ae3abd6d2ad23256#file-game-after-detect-keypress-func-html).

### Creating the Game Loop Function

The game loop function, or `update` function, will run many times per second and each time it runs it will calculate the new position for all the colored dots, and also check to see whether the player is over one or not. Then it will clear the canvas and redraw the scene. 

It is important to clear the canvas each time or else every pixel that is drawn will just layer on top of the previous ones, creating an ugly mess where there is are a ton of dots overlapping one another.

Before you start, though, you'll need to create some more variables, including one type that you haven't learned yet.

##### JavaScript lesson: Arrays

Arrays are simply lists of variables, all stored withing another variable. 

For example, say you have a list of ages that you need to store in variables. You're not exactly sure how long the list will be, but even if you did, it would be very long. So obviously 

    let age1 = 16;
    let age2 = 87;
    let age3 = 33;
    let age4 = 12;
    ...
 
is not an option. The solution is to use an array, which can store any amount numbers, strings, or other objects in a way that you can easily access them but they _don't all have to have variable names_. Array syntax is simple: `let my_array = [values, separated, by, commas]`.

Here's how you would store the above in an array:

    let ages = [16, 87, 33, 12, ...];

And then you can access the ages with the syntax `ages[i]`, where `i` is an index that identifies what value you want. Because **in programming, counting always starts with `0`**, to get the third value in the array you'd use an index of `2`:

`ages[2]` is equal to `33`.

I'll teach more about arrays later.

#### Creating some variables



##### JavaScript lesson: `for` loops

`for` loops are most commonly useful in two scenarios: When you want some code to run a certain number of times, or else when to iterate over every item in an array and do something to each one.

The basic syntax for a for loop is `for (before loop starts; condition to continue; after each iteration) {code block; do something;}`. Here's an explanation:

A for loop runs the code in the code block (the code between `{` and `}`) over and over again **as long as** the condition, or the second statement in the parenthesis, is _true_. 

At the beginning of each loop, the `for` loop checks the condition - if it is true, then the code block is executed. If it is false, then the code block is not executed and the program continues with the rest of the code. Whenever the code block is finished running, the `for` loop executes the third statement inside the parenthesis and then goes back to the beginning and checks the condition again.

Before starting the first loop, the `for` loop will execute the first statement in the parenthesis. The third and last statement in the parenthesis will be executed _every time_ after the end of the loop, before going back to the beginning. 

Here's an example of using a `for` loop to run some code 5 times, and then using a `for` loop to iterate through every item in an array:

<script src="https://gist.github.com/scitronboy/d8bdf792743b12f2388f2f1fb1170297.js"></script>




























    





























