---
title:  "About this blog"
date:   2020-01-20
categories: Blog
tags:
  - about
  - blog
  - website
  - jekyll
image: bg.jpg
slug: about-this
---
Hi! Welcome to my website. I'm Diego, a software engineer currently living in Chicago. I've had the idea of creating a blog and a personal page in my mind for a really long time, and here they are at last, 2 in 1! The next step is to actually create useful content regularly and don't abandon the project.

The scope of this blog is going to be mostly technical, and the main idea is to post about things I consider useful or solutions to problems I face and I may face again in the future. My interests (and what I may post the most about) are software development, Raspberry Pi and other single board computers, retro gaming and home automation.

As this is a blog about tech stuff, let's start with some info about how I did this site.

## Technical info about the blog
The blog is hosted in [github pages](https://pages.github.com/), which allows to host static websites. To generate the website I used [Jekyll](https://jekyllrb.com/). Jekyll is a static-site generator (SSG) focused on websites and blogs. There are other SSG like [Gatsby](https://www.gatsbyjs.com/), [Hugo](https://gohugo.io/) or [Ghost](https://ghost.org/). I did some research about the differences but they seemed to offer more or less the same for what I needed, so I went with Jekyll, which is the one suggested by github pages. The idea behind these kind of software is to generate sites without any backend code or database, so every time a change is made, ~~the files affected by those changes~~ all the files are regenerated. Posts and content in general can be created using markdown or html (there may be others, but these two are the most common), which makes it really easy to start writing once you have everything ready.

### Themes
One of the good things of these SSG is that there are already lots of themes and you can customize them if you know some web development. And if you don't need something fancy, you can just get one of the themes and start writing. I didn't need something fancy, but I liked things of two themes, so I ended mixing both of them. This can be more or less easy if you do this for different layouts inside your blog, but I wanted some fancy mixing (the index page is a mix of the themes), so I had to make some major tweaks (I don't recommend looking at my changes in the css).

The two themes I used are:

#### Uno theme
Before I decided to use themes, I started working on a design for my **personal website** in something that was similar to this one, with some extras (projects section), so I decided to use it and discard my previous progress. I used a modified version of this theme by [Thomas Zühlke](https://github.com/tzuehlke) ([repo](https://github.com/tzuehlke/jekyll-uno-timeline)) and then I made some more changes on top of it.

![Uno theme screenshot](https://user-images.githubusercontent.com/32843441/72224870-5451ff00-357f-11ea-8fc2-bfbd4499bc63.gif)


#### Casper theme
This is not the first time I've worked on starting a **personal blog** (though it is the first time that I actually published it). The other time was around 2012-2013 and my idea was to use Ghost, hosted in a Raspberry Pi. One of the things I liked from Ghost was the default theme (called Casper, I hope everyone got the reference), so I decided to find a similar theme for the Blog part of the website. Luckily, popular themes are in all platforms, so it wasn't hard to find a Jekyll theme based on it (on the first version of Ghost, there are two newer versions, which I didn't like as much). The theme I used as a base for this is [this one](https://github.com/rosario/kasper) by [Rosario Rascuna](https://github.com/rosario). Looking back, I think I should've also used this as the main theme for the index/cover and add Uno changes on top of it.
![Casper theme screenshot](https://jekyll-themes.imgix.net/images/themes/jasper-jekyll-theme.jpg?w=1140&h=713&fm=webp&fit=crop&crop=top)

This is all for now!

