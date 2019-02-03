---
layout: post
section-type: post

date: 2019-02-03
#update-text: 
#lastmod: yyyy-mm-dd

title: "Make a fun HTML & JavaScript game - part 1: HTML tutorial"
image: "/img/post/js-game/game.jpeg"
top-image: "/img/post/js-game/js-html-game.jpeg"
slug: "html-js-game-part1"
category: "code"
tags: [ 'web-dev', 'game', 'code', 'project', 'tutorial' ]

postdescription: "Learn how to create a simple but fun online browser-based game with HTML and JavaScript. Part 1: HTML tutorial - learn how the language that makes webpages possible works."

titletag: "Make a fun HTML & JavaScript game - part 1: HTML tutorial"
description: "Learn how to create a simple but fun online browser-based game with HTML and JavaScript. Part 1: HTML tutorial - learn how the language that makes webpages possible works."
---

![the game](/img/post/js-game/js-html-game.jpeg "The game")

In this tutorial series, I will guide you through the steps to create your own simple but fun online browser-based HTML game, using HTML and JavaScript. 

This tutorial is meant for beginners (or almost-beginners), but I will only cover what is needed to teach you how to make this game, so I will include links at the bottom so you can learn the topics in greater detail. Keep in mind that this is a very big project to use as a first HTML project, so if it seems too complicated, just learn more about the topics and come back when you understand them better.

I will cover the following topics in this tutorial:

+ I will explain HTML and what it is
+ I will go over basic HTML syntax
+ I will show how to create a simple webpage to house the game
+ I will show how to use the HTML canvas element to create the game graphics
+ I will teach JavaScript basics
+ I will show how to interact with the canvas and generate graphics using JavaScript
+ I will show how to get user input to control the player
+ I will show how to use cookies to create a high score system
+ And more!

When finished, the game is a game in which the goal is to fly over as many colored dots as possible before it ends. 

The first part (this article) of this tutorial covers the basics of HTML. Have fun and make sure you leave a comment at the bottom if you have any questions!

### Intro to HTML

HTML, as you probably already know, is the language that makes web pages possible. Every web page on the internet, excluding images, plain text files, and other "static" files that are not web pages on their own, is really just a file containing HTML. That HTML is what tells your browser how the web pages should look, and tells it what images, links, sounds, videos, etc. should be on the web page. 

So when you type a URL into your browser, or click on a link, your browser sends a request to the server of the website and tells it what page it wants. The server then processes your request and responds by sending a HTML file back to your browser. Your browser reads the HTML file to know what the web page should look like, and then it "renders" the web page onto your screen.

Some people think that HTML is a "programming language," but it's really not. It's actually a "markup language" (the _ML_ in HTML stands for Markup Language). The difference is that a programming language is a set of instructions for a computer to follow, usually line-after-line, that tells it how to do something (like rendering a markup file). Whereas a markup file is just a file of text that includes extra little bits that tell your browser (or whatever is reading the markup) how to structure and display the text.

Now, you might be wondering how websites can be interactive or dynamic or have games, etc., if they don't have any code that tells the browser what to do. (If you _were_ wondering that, that's great because it means you probably completely understand the concept of markup language vs programming language.) Well actually, web pages do have a programming language, it's just not HTML. Websites can be made dynamic or given interactive elements or other functionality by adding _JavaScript_ to them (do __NOT__ confuse JavaScript with Java. They are completely different languages!). JavaScript can be included in a special part of the HTML, or else it can be imported from a different file. Where ever it is loaded, the browser reads it and follows the instructions within, which can be instructions to modify text, add elements, change sizes or colors of elements on the page, add animations, and much more. In part two of this tutorial, we will need JavaScript to make the game itself.

One last thing to know about HTML is that HTML, on its own, cannot "style" websites, or make them look pretty or decorated or anything else - HTML is just used to define the layout and everything on the page. For styling a page, you must use CSS (Cascading Style Sheets). CSS, like JavaScript, is either included in a special place in the HTML or else imported from another file. To see how important CSS is, try [viewing this page without any CSS or javascript](/page/no-css-page)). I will not cover CSS in this tutorial.

Since the purpose of this tutorial is to make a JavaScript game, I will only cover a tiny bit of HTML in this tutorial (I will also only cover a tiny amount of JavaSCript). If you want to learn more, I will include links to more resources below.

#### Requirements and Setup

Doing basic HTML development is very easy because the only tool that you really need is your web browser. Also, it is much nicer to use a real text editor (code editor) for programming, because:

+ It will syntax-highlight, or color, your code. This is nice because you can easily find different parts of the code, and it makes programming much easier.
+ Most have special auto-complete and code correction features that make writing code easier.
+ They make sure that the formatting (new lines, spacing, indentation, etc.) works properly.
+ They use fixed-width code fonts that make code easy to read and ensure that you don't, for example, confuse an "O" with a zero.
+ They have side-bars that make opening code files easy.
+ They have a lot of other features that will make writing code much more fun.

I recommend [Atom](https://atom.io/). If you can't get that to work, there are other options such as Kate for linux or Sublime text for Mac.

Once you have a text editor, create a new folder and name it whatever you want. Then, create a new file in that folder and name it `game.html` (or whatever you want to call it.) Open the file in your code editor so that you can edit it.

Then open up your browser and hit `Ctrl+o`, or `Cmd+o` on a mac. This should bring up a window where you can specify a file to open. Navigate to the file you just created and open it. It will be blank obviously because we haven't added anything to it yet. Let's change that!

### Basic HTML syntax

HTML is quite simple. Elements (pieces of content) in the code are put between what are called "tags" and the tag names tell the web browser how the piece of content should look, and what it is. For example:

> `<tagname>content</tagname>`

You can see that there are two tags, a starting tag and an ending tag. The starting tag is just the tag name surrounded by angle brackets. The ending tag is the same but it has a forward slash before the tag name. 

There are many tags in HTML. You would italicize or embolden something in HTML using the `<i>` or `<b>` tags:

> `This is plain text. <i>This is italicized.</i> <b>And this is in bold.</b>`

Try adding that to your file `game.html`. Then reload the browser window and you should see something like this:

> This is plain text. _This is italicized._ **And this is in bold.**

You can see that the browser just uses the tags to determine how the text should be displayed. It doesn't show the tags themselves. This is basically how HTML works - you put tags around content to tell the browser what to do with it. 

Congratulations! You may have just written your first piece of HTML! 

You can also nest tags within each other:

> `This is plain text. <b>And this is in bold. <i>And this is in bold and italicized.</i></b>`

will look like this:

> This is plain text. **And this is in bold. _And this is in bold and italicized._**

because the last two sentences are both within the `<b>` tags, so they are both bold, but the last sentence is also within a `<i>` tag so it is also italicized.

#### HTML Attributes

Often, tags themselves do not give the browser all the information you want them to. When this happens, you can use what are called _HTML attributes._ HTML attributes are special additions to tags that give the browser additional information about a specific tag. For example, if you want an image to have a specific size, you might use the `width ` and `height` attributes. HTML attributes always come after the tag title _within_ the starting tag, and they are written in the form `attribute="value"` - make sure you always put quotes around the value. Here's an example:

> `Hello! <a href="https://www.google.com/">Click here to go to Google</a>`

That example was actually a link. To make links in HTML, you just surround the link text with `<a>` tags and then you _must_ add an attribute called `href` that tells the browser _where the link should go_. So if you click on that link, it would send you to `https://www.google.com/`:

> Hello! [Click here to go to Google](https://www.google.com/)

There are many other uses for HTML attributes. I will show you more later on.

#### HTML comments

When you are writing code (within markup or a programming language), sometimes you will want to add little notes to yourself or others. The easiest way to do this is by adding them to the file, but if you did that, then the browser would read your comment, which you don't want it to do. Luckily, HTML (and other languages) actually has a feature which you can use to tell the browser to _ignore_ the line. When you add a comment to a HTML file, the browser will completely skip the line and pretend it didn't exist. In HTML, you can add comments using the `<!--` and `-->` tags (don't get confused - the syntax is different for comments than for other tags, but that's just the way it is). Here's an example:

> `This is text that the<!-- It will ignore this part --> browser will render`

will appear like this:

> This is text that the browser will render

I will use comments throughout the rest of this tutorial when I need to explain pieces of code.

Now, we are going to finally start the project.

### Creating the webpage

Here is the HTML page that our game will live in. Copy it into your `game.html` file and then see if you can figure it out:

<script src="https://gist.github.com/scitronboy/d5f5017fadffcdb0dc3906cef590a45d.js"></script>

##### Explanation:

The file starts with the line `<!DOCTYPE html>`. This is unrelated to the actual content, but it just tells the browser that the file is written in HTML, or specifically HTML5. Make sure you include it at the top of every HTML file you write.

The rest of the file is a ton of stuff enclosed within `<html>` tags. Obviously this just tells the browser that everything within the tags is the HTML.

Then we have one line enclosed within `<head>` tags. This is a bit mysterious, but it is mainly just an organization thing for HTML (by "organization thing", I do not mean it's unimportant). Usually, you put stuff that is not actually part of the page content in the head, such as useful information for the browser: analytics scripts, styling and CSS information, icons, descriptions for search engines, off-page titles, etc. 

The only thing that is important for us to put in the head section is a title that tells the browser what the page is called. Most browsers will use this to add a title to the top of the tab, or window, like this:

![tab title](/img/post/js-game/tab_title.png "tab title")

Next comes the `<body>` section, which contains all the page content. First, we have a `<h1>` (header 1) tag, which creates a primary header for the page:

![page header](/img/post/js-game/page_title.png "page title")

Then we have a `<p>` (paragraph) tag, which adds a sentence to the page:

![page sentence](/img/post/js-game/page_p.png "page sentence")

And then we have something called a "canvas." The canvas is the most important thing on the page, along with the script. The `canvas` tag was added to HTML in HTML5, and it just creates an area where you can use JavaScript to "draw" on it, like a canvas. In the next part of this tutorial, I will show you how we can use JavaScript to create the graphics (visual parts) of the game and draw them onto the canvas. 

You might notice that the canvas has two HTML attributes but it does not have anything inside it (between the starting and ending tags). Two of the attributes are `width="400"` and `height="400"`. These just tell the browser that the canvas needs to be 400 by 400 pixels in size. There is also a `id="c"` attribute. `id` attributes are used in HTML to mark, or identify, objects. When you give an element an ID, you can then find that element in JavaScript or CSS using the ID. In the next part, the JavaScript will use the id to find the canvas to draw on. The canvas element is empty because we do not need to "add" anything to it - it is just an empty canvas to draw on. 

The last thing in the file is a `<script>` section. Browsers treat stuff inside `<script>` tags as JavaScript, and run them as soon as they get to that part of the file. Because of this, you can't put anything but JavaScript inside a script section because it will cause a JavaScript error. That's why the comments inside the script tags use two double slashes: JavaScript comments are different than HTML comments.

### Conclusion + More resources

That's the end of the HTML part. In the next parts, I will go over the basics of JavaScript and teach you how to make the game using the webpage you just created. It will be released some time in the next few days.

Here are a few resources to learn more about HTML:

+ [W3 schools official HTML tutorial](https://www.w3schools.com/html/html_intro.asp)
+ [HTML.com](https://html.com/)
+ [CodeCademy HTML tutorial](https://www.codecademy.com/learn/learn-html)
+ [W3 schools HTML & JavaSCript game tutorial](https://www.w3schools.com/graphics/game_intro.asp)

If you need help or have any suggestions please leave a comment below or else [contact me](/#contact). Thanks for reading!











