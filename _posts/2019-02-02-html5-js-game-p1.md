---
layout: post
section-type: post


#date: yyyy-mm-dd
#update-text: 
#lastmod: yyyy-mm-dd

title: "Make a fun HTML & JavaScript game - part 1: HTML tutorial"
top-image: "/path/to/bannerimage"
slug: "html-js-game-part1"
category: "code"
tags: [ 'web-dev', 'game', 'code', 'project', 'tutorial' ]

postdescription: description of post for site

titletag: "Make a fun HTML & JavaScript game - part 1: HTML tutorial"
description: <meta> description
---

In this tutorial series, I will guide you through the steps to create your own simple but fun online browser-based game, using HTML and JavaScript. 

This tutorial is meant for beginners (or almost-beginners), but I will only cover what I need to to teach you to make this game; so I will include links in each section so you can actually learn the topics in greater detail. Keep in mind that this is a very big project to use as a first HTML project, so if it seems to complicated, just learn more about the topics and come back when you understand them better.

When finished, the game will 

The first part (this article) of this tutorial covers the basics of HTML.

### Intro to HTML

HTML, as you probably already know, is the language that makes web pages possible. Every web page on the internet, excluding images, plain text files, and other "static" files that are not web pages on their own, is really just a file containing HTML. That HTML is what tells your browser how the web pages should look, and tells it what images, links, sounds, videos, etc. should be on the web page. 

So when you type a URL into your browser, or click on a link, your browser sends a request to the server of the website and tells it what page it wants. The server then processes your request and responds by sending a HTML file back to your browser. Your browser reads the HTML file to know what the web page should look like, and then it "renders" the web page onto your screen.

Some people think that HTML is a "programming language," but it's really not. It's actually a "markup language". (the ML in HTML stands for Markup Language) The difference is that a programming language is a set of instructions for a computer to follow, usually line-after-line, that tells it how to do something (like rendering a markup file). Whereas a markup file is just a file of text that include extra little bits that tell your browser (or whatever is reading the markup) how to structure and display the text.

Now, you might be wondering how how websites can be interactive or dynamic or have games, etc., if they don't have any code that tells the browser what to do. (If you _were_ wondering that, that's great because it means you probably completely understand the concept of markup language vs programming language.) Well actually, web pages do have a programming language, it's just not HTML. Websites can be made dynamic or given interactive elements or other functionality by adding _JavaScript_ to them (do __NOT__ confuse JavaScript with Java. They are completely different languages.). JavaScript can be included in a special part of the HTML, or else it can be imported from a different file. Where ever it is loaded, the browser reads it and follows the instructions within, which can be instructions to modify text, add elements, change sizes or colors of elements on the page, add animations, and much more. In part two of this tutorial, we will need JavaScript to make the game itself.

One last thing to know about HTML is that HTML, on its own, cannot "style" websites, or make them look pretty or decorated or anything else - HTML is just used to define the layout and everything on the page. For styling a page, you must use CSS. CSS, like JavaScript, is either included in a special place in the HTML or else imported from another file. To see how important CSS is, try [viewing this page without any CSS or javascript](/page/no-css-page)). I will not cover CSS in this tutorial.

Since the main point of this tutorial is to make a JavaScript game, I will only cover a tiny bit of HTML in this tutorial (I will also only cover a tiny amount of JavaSCript). If you want to learn more, I will include links to more resources below.

#### Requirements and Setup

Doing basic HTML development is very easy because the only tool that you really need is your web browser. Also, it is much nicer to use a real text editor (code editor) for programming, because:

+ It will syntax-highlight, or color, your code. This is nice because you can easily find different parts of the code, and it makes programming much easier.
+ Most have special auto-complete and code correction features that make writing code easier.
+ They make sure that the formatting (newlines, spacing, indentation, etc.) works properly.
+ They have side-bars that make opening code files easy.
+ They have a lot of other features that will make writing code much more fun and easy and less frustrating.

I recommend [Atom](https://atom.io/). If you can't get that to work, there are other options such as Kate for linux or Sublime text for Mac.

Once you have a text editor, create a new folder and name it whatever you want. Then, create a new file in that folder and name it `game.html` (or whatever you want to call it.) Open the file in your code editor so that you can edit it.

Then open up your browser and hit `Ctrl+o`, or `Cmd+o` on a mac. This should bring up a window where you can specify a file to open. Navigate to the file you just created and open it. It will be blank obviously because we haven't added anything to it yet. Let's change that!

#### Basic HTML syntax

HTML is quite simple. Elements (pieces of content) in the code are put between what are called "tags" and the tag names tell the web browser how the piece of content should look, and what it is. For example:

> `<tagname>content</tagname>`

You can see that there are two tags, a starting tag and an ending tag. The starting tag is just the tag name surrounded by angle brackets. The ending tag is the same but it has a forward slash before the tag name. 

There are many tags in HTML. You would italicize or emphasize something in HTML using the `<i>` or `<b>` tags:

> `This is plain text. <i>This is italicised.</i> <b>And this is in bold.</b>`

Try adding that to your file `game.html`. Then reload the browser window and you should see something like this:

> This is plain text. _This is italicised._ **And this is in bold.**

You can see that the browser just uses the tags to determine how the text should be displayed. It doesn't show the tags themselves. This is basically how HTML works - you put tags around content to tell the browser what to do with it. 

Congratulations! You may have just written your first piece of HTML! Now, we are going to finally start the project.

### Creating the webpage

In this part I will explain how to create the webpage that the game will live in. First, I will quickly explain _comments_.

#### HTML comments

When you are writing code (wither markup or a programming language), sometimes you will want to add little notes to yourself or others. The easiest way to do this is by adding them to the file, but if you did that, then the browser would read your comment, which you don't want it to do. Luckily, HTML (and other languages) actually has a feature which you can use to tell the browser to _ignore_ the line. When you add a comment to a HTML file, the browser will completely skip the line and pretend it didn't exist. In HTML, you can add comments using the `<!--` and `-->` tags (don't get confused - the syntax is different for comments than for other tags, but that's just the way it is). Here's an example:

> `This is text that the<!-- It will ignore this part --> browser will render`

will appear like this:

> This is text that the browser will render

I will use comments throughout the rest of this tutorial to explain pieces of code.

#### Layout of the HTML page

Here is the HTML page that our game will live in. Copy it into your `game.html` file and then see if you can figure it out:

<script src="https://gist.github.com/scitronboy/d5f5017fadffcdb0dc3906cef590a45d.js"></script>

##### Explanation:

The file starts with the line `<!DOCTYPE html>`. This is unrelated to the actual content, but it just tells the browser that the file is written in HTML, or specifically HTML5. Make sure you include it at the top of every HTML file you write.

The rest of the file is a ton of stuff enlosed within `<html>` tags. Obviously this just tells the browser that everything withing the tags is the HTML.

Then we have one line enclosed within `<head>` tags. This is a bit mystrerious, but it is mainly just an organization thing for HTML (by "organization thing", I do not mean it's unimportant). Usually, you put stuff that is not actually part of the page content in the head, such as useful information for the browser: analytics scripts, styling and CSS information, icons, descriptions for search engines, off-page titles, etc. 

The only thing that is important for us to put in the head section is a title that tells the browser what the page is called. Most browsers will use this to add a title to the top of the tab, or window, like this:

![tab title](/img/post/js-game/tab_title.png "tab title")

Next comes the `<body>` section, which contains all the page content. First, we have a `<h1>` (header 1) tag, which creates a primary header for the page:



Then we have a `<p>` (paragraph) tag, which adds a sentence to the page:



And then we have something called a "canvas." The canvas is the most important thing on the page, along with the script. The `canvas` tag creates 












