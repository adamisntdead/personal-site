---
title: 'A Look At WebAssembly '
date: '2018-07-01T00:00:00+01:00'
excerpt: >-
  I gave a talk at DublinJS about WebAssembly, about its performance, when to
  use it and more.
comments: true
noauthor: false
math: false
share: true
categories: speaking
---
> My talk for the DublinJS Meetup, [June 2018](https://www.meetup.com/DublinJS/events/fbllfpyxjbhb/) ⚡️

**Description:** In this talk I will be looking at WebAssembly and what it can do for you. I will speak about the strong and weakpoints of using it in your apps, along with some things to keep in mind performance wise. Lastly, I will show a few resources and tools which make using WebAssembly in your apps and learning more about it easier

**Slides:** Available on SlideShare

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='//www.slideshare.net/slideshow/embed_code/key/1zRFqy5Vv7abMk' frameborder='0' marginwidth='0' marginheight='0' scrolling='no' style='border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;' allowfullscreen> </iframe> <div style='margin-bottom:5px'> <strong> <a href='//www.slideshare.net/AdamKelly76/a-look-at-webassembly' title='A Look At WebAssembly' target='_blank'>A Look At WebAssembly</a> </strong> from <strong><a href='https://www.slideshare.net/AdamKelly76' target='_blank'>Adam Kelly</a></strong> </div></div>

## Takeaways

1. WebAssembly is best used for speeding up a small, performance intensive part of your app
2. You can write WebAssembly raw, but it is hard, and much better to use higher level languages.
3. Rust, C and C++ have first class tickets in the WebAssembly world
4. There is no garbage collector, JIT or intermediate representation, which is some of the reasons why it is so fast
5. WebAssembly can give speeds of up to 80% of native performance
6. It runs in a sandboxed environment, and memory addresses are remapped to a typed array which can also be accessed from the javascript side
7. Currently, DOM maniputations (and calling other related functions) have a bit of overhead, so loads of small function calls will give a performance loss, it is best to do everything on one side, and only passing between Webassembly and Javascript when needed.
8. There is only support for numeric types, and tools like [wasm-bindgen](https://github.com/rustwasm/wasm-bindgen) can give extra types such as javascript objects.
9. You should check out [WebAssembly Studio](http://webassembly.studio/)!

## Demo

I also gave a demo, of a small markov chains implementation using WebAssembly and Rust!

![Demo Image](https://cdn.rawgit.com/adamisntdead/wasm-markov/4e072f11/screenshot.png)

You can see the source code on [GitHub](https://github.com/adamisntdead/wasm-markov)
