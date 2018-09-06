---
layout: post
section-type: post

title: "Easy Blogging for Programmers: Static Site Generators"
titletag: "Static Site Generators"

description: "What is a static site generator? What are the benefits of using a static site generator? Learn how static site generators work and why you should use one. Also, learn how to use the jekyll ssg with github pages."

keywords: "static site generator, static site generators, ssg, why use a static site generator, what is a static site generator, github pages, what is github pages, static website, static site generators review, static, jekyll, blog, static site generators 2017, static site generators 2016, static site generators 2018"

slug: "static-site-generators"
category: "Blog"
tags: [ 'jekyll' ]
---

Suppose you want to create a blog. But you don't want to use one of those drag-and-drop, Word-style, CMS (content management system) programs like [Wordpress](https://wordpress.org/) or [Blogger](https://www.blogger.com/), because first of all you want more control and customization, and second of all you want to program your site, not just design it. You have several options.

You could program the whole site, with a backend, using a framework in some language like python, java, go, or C - but writing a whole backend _and_ frontend just for a simple _blog_? Overkill. 

Or you could write a 100% static site - but this is obviously not a reality for a blog, because every time you wrote a new post, you'd have to go back and add the links for that post to the archive pages and such. You _could_ do this in a scrape, but it wouldn't be very flexible.

So, what do you do when you need a fast, simple blog that you can easily customize (in code), and you don't want to use a CMS or worry about a database, server, and security? One option is to use a _static site generator_. 

A static site generator takes in dynamic code, usually a combination of HTML/CSS/Javascript, some templating language (Jekyll uses [Liquid](https://shopify.github.io/liquid/)), and some kind of markdown language, and generates a static site with it. So, for example, when you generate your site, you can write templating code for the archive pages that will generate a link for _every_ post in, for example, a posts folder.

The most popular open source static site generator by far is [_Jekyll_](https://jekyllrb.com/). A huge [list of sites made with jekyll](https://github.com/jekyll/jekyll/wiki/Sites) can be found here. I made this blog with jekyll.

Here are some advantages of using Jekyll or [another static site generator](https://www.staticgen.com/): 
<div>
<ul style="text-align: left">
<li><strong>Ease.</strong>
You want a customizable, static site, but that doesn't mean you want to write every page in raw HTML - Jekyll uses <a href="https://en.wikipedia.org/wiki/Markdown">Markdown</a> language to make writing blog posts as easy as writing an email, even for non-programmers.
</li>
<li><strong>Simplicity.</strong>
Jekyll is extremely simple - you won't get bogged down in hundreds of settings, dependencies, etc.
</li>
<li><strong>Speed.</strong>
Jekyll generates amazingly fast sites because static sites don't need to query databases or render templates - they just return whichever page you request. Because of this, static sites don't crash as often as dynamic sites.
</li>
<li><strong>Security.</strong>
Because static sites don't make database querys, they are extremely secure.
</li>
<li><strong>Easier to Maintain.</strong>
Static sites don't have servers with databases to maintain, because you can easily, simply host them on services such as github pages or Amazon S3. Because of this, static sites are also much cheaper.
</li>
</ul>
</div>

The only major downsides to using static sites are they can't accept user input/login, because there is no database to store that information. This means no forms, comments, logins, etc. However, you can get around this by using [external services](https://jekyllrb.com/docs/resources/#integrations) like [Disqus](https://disqus.com/) (for commenting) and [Formspree](https://formspree.io/) (to create forms).

There is one last major reason why I think Jekkyl is so great (and why I chose to use it): Jekyll is the official engine that [Github Pages](https://pages.github.com/) uses. Github Pages is a free hosting service for static sites, and until I become a little more serious about my blogging, I plan to continue to host my site using Github Pages. 

Because github pages already uses jekyll, all you need to do to publish (or update) a Jekyll site to github pages is push your source code to your github repository. and by naming that repo scitronboy.github.io, github automatically serves the site, using Jekyll, to [scitronboy.github.io](https://scitronboy.github.io/) :grinning:.

If you're a programmer looking to create an easy, simple, blog, I highly recommend [Jekyll](https://jekyllrb.com/) and [Github Pages](https://pages.github.com/).


_ps. I am planning to write a tutorial on creating a jekyll blog sometime in the future._
