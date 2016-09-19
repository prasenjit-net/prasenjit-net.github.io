---
layout: post
title: "How I created this blog"
date: 2016-09-19 17:38:41 -0530
categories: test
---

In present days blog is a very effective way to express the thoughts created inside mind. I am also starting to explore 
this medium to see how this can be dome. My first post in this blog is also about the same. Explaining `How I created this 
blog`.

### What is being used

* Github Pages for hosting
* Jekyll blog-aware static content generator
* Liquid HTML templating
* Markdown text markup for post writing
* Textile text markup maybe used for post writing but not using now
* Bootstrap CSS library for layout

### How it works

In this setup `Github Pages` is the web hosting provider. `Github` gives very good web hosting service free of cost from within any 
`Github` repository. Nothing special is required to host some static pages from `Github`. A good 
[documentation](https://help.github.com/categories/github-pages-basics/) is available from `Github`, which is also served from `Github Pages`. 
Only concern/limitation for `Github Pages` is that it can **only host static pages**, i.e. no server side logic, database ect. This could be a 
limitation for someone but may not for a lot of people. But there is an advantage of being static only pages that there is no challenge 
of scaling the site to any capacity, infact that is completely abstracted by `Github Pages`. What you only need to do is to push your 
static pages to some repository and expose that through `Github Pages`.

Jelyll here is a framework for static page generator, it can transform `Markdown` or `Textile` documents to static `HTML` pages for publish. 
Github has support for jekyll and can internally run the build and generate the static files by itself. So one more problem is solved this 
page generation also doesnt require to build. Neither it require any kind of CI/CD.

Jekyll uses `Liquid` HTML template to pre-generate all HTML files during build time. But as build is also taken care by Github, it almost feels 
like, yu are writing some dynamic HTML templating and pages are generated while being served. For a complete documentation of 
Jekyll [this](https://jekyllrb.com/docs/home/) site can be visited. However gor a better getting started guide go to [Github's Jekyll 
help pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/).

`Markdown` is used for structured markup writing. It can ease the pain of structuring your written document. Easily layout the writing 
according to a standard HTML tree.