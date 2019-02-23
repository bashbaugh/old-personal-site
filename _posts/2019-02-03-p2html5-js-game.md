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

![banner image](/img/post/js-game/js-html-game.jpeg "js + html")

In this tutorial series, I will guide you through the steps to create your own simple but fun online browser-based game, using HTML and JavaScript. 

This is part two of the tutorial. **If you have not read part one, you should read it now.** [Click here for part 1: HTML tutorial](/code/2019/html-js-game-part1). Once you have read it, you can continue with this part.

In this part of the tutorial, I will show you how to add JavaScript to program the game itself. Keep in mind that this tutorial is meant for beginners (or almost-beginners), but I will only cover what is needed to teach you how to make this game. It may seem to complicated or hard at first, so if it does check out some of the links at the bottom and come back when you feel a little more comfortable with JavaScript basics. 

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

`let my_name = "Benjamin";`

You can see in the last example that I didn't name the variable `my name`, I named it `my_name` with an underscore instead of a space. This is very important because **variable names cannot have spaces**.

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

_One last note: the `log` command is a function, however the `console` part is not actually part of the function name. It is instead a "class" that houses the `log` function. I will not cover classes in this tutorial, but just remember that function names cannot actually have periods in them. Remember that when you start to create your own functions._

























