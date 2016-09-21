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
according to a standard HTML tree. As an example this page is also written in Markdown, then transformed to HTML by Jekyll. A complete 
documentation about Markdown can be found [here](https://guides.github.com/features/mastering-markdown/).

Liquid can be used like any normal serve side page rendering engine but for static generation only. It does not have any access to any 
parameter available at runtime. Like it does not read query parameter. A limited support of pagination can be used with jekyll and Liquid 
but all pages are generated during build of the project.

I did not mention about it in 'What's being used' section, but Ruby is also used at the backbone of this project. But you really dont need 
to know about Ruby to work with it. Honestly i dont know anything about Ruby but still i can run this. One thing i can tell you, if you know 
Ruby and can setup your dev environment, it is much easy to see the layout of your site well before it goes live.

### How I made it work for me

#### First Github repository

Create the repository under your username or organization. Follow the rules of Github pages for hosting a user page or organization page. 
You need the name the repository as `<username>.github.io` or `<orgname>.github.io`.

#### Setup a dev environment

Preferably in a linux machine. Like i did in a Centos minimal VM. Make sure you are connected to the internet.

First install `Ruby` development edition along with few other libraries with command. You should at least have Ruby version 2.0.0 or above.

```bash
# install Ruby git and some other required packages
$ sudo yum install gcc-c++ libffi-devel zlib-devel ruby-devel git
```

```bash
# check Ruby version
$ ruby --version
```

Then install Ruby Gem `bundler` from Gem installer

```bash
# install bundler for Ruby package management
$ gem install bundler
```

Now clone your empty repository into a local folder. And switch the required branch as per the convention. Branch `master` for user or 
organization site, and `gh-pages` for project site. This is configurable too.

```bash
# clone repository
$ git clone <gihub repo url>
# create branch if it is project site
$ git checkout -b gh-pages
```

#### Create project files

Now create a file named `Gemfile`. This will hold the Ruby gem configuration for the project.

```bash
$ cd <project-dir>
$ touch Gemfile
```

Now edit the file in editor and put below files in there.

```ruby
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```

Now run the below bundler command to automatically install all required Gems along with Jekyll.

```bash
$ bundle install
```

At this point the project is ready but without any source file. Now to generate a scaffolding execute the below command.

```bash
$ bundle exec jekyll new . --force
```

This will override the Gemfile file. Now open it in editor and comment, uncomment or add as the snipped below.

```ruby
# comment
#gem "jekyll", "3.2.1"

# uncomment
gem "github-pages", group: :jekyll_plugins

# add
gem "therubyracer"
```

The last one is added to enable dev server with a javascript engine. This one is not required if node.js is installed. 
After this your site is ready. You can commit the files generated and push to see it go online. At 
https://username.github.io or https://orgname.github.io as per your configuration.

To run the dev server execute the command. You can see your site locally with some browser.

```bash
$ bundle exec jekyll serve
```

### Customize Layout

There are many theme available for Jekyll, but Github officially support only minima theme. So if you want to use other theme 
You either need to customize it or build the site by yourself from `master` branch and push the generated files to the `gh-pages` 
branch. This is not easy as every time all files will be modified and pushed to Github. But you can use a CI/CD for that and Travis is 
best way to go. For example see https://github.com/prasenjit-net/jekyll-asciidoc-quickstart.git. But customizing will give you much more 
flexibility.

Now you can see the auto generated site does'nt look good as per the modern web site layout. To start customize your layout 
you need to have a starting point, for that you can import the files available in theme minima and start from there. To 
do that follow instructions below

```bash
# Find the file location of minima
$ bundle show minima
/usr/local/share/gems/gems/minima-1.2.0
# copy the files from there
$ cp -r /usr/local/share/gems/gems/minima-1.2.0/_layout .
$ cp -r /usr/local/share/gems/gems/minima-1.2.0/_includes .
$ cp -r /usr/local/share/gems/gems/minima-1.2.0/_sass .
```

Now you have a starting point. Go and have a look at those files to understand the structure of the 
files. You can use any theme available with you or create one with `bootstrap`. For a reference have a look at my 
project at https://github.com/prasenjit-net/site.git to understand more about layout.