---
title: console.md - Rendering Markdown In The Javascript Console
date: '2018-07-01T13:14:25+01:00'
image: /images/screen-shot-2018-07-01-at-1.28.46-pm.png
excerpt: My lightning talk for the May DublinJS meetup
comments: true
noauthor: false
math: false
share: true
categories: speaking
---
> My lightning talk for the DublinJS Meetup, [May 2018](https://www.meetup.com/DublinJS/events/fbllfpyxhbcb/)

So I got the opportunity to give a lightning (5 minute) talk at [DublinJS](https://www.meetup.com/DublinJS) about a little thing I made called `console.md`, which takes a subset of markdown, styles it, and outputs it to the browser console.

At the moment, it supports:

* All headings
* Standard text
* Bold text
* Inline code
* Block code (although not highlighted)
* Images (only at full size)
* Links (kind of)

And it outputs something like this:

![output](https://raw.githubusercontent.com/adamisntdead/console.md/master/media/output.png)

If you want to play around with it, check out the source code on [GitHub](https://github.com/adamisntdead/console.md), and the slides below.

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe title="slides" src='//slides.com/adamkelly-2/console-md/embed?style=light' scrolling='no' frameborder='0' webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>
