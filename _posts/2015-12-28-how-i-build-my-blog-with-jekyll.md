---
layout: post
title:  "How I Build My Blog With Jekyll"
date:   2015-12-28
categories: jekyll
---

[Jekyll](https://Jekyllrb.com/) is a static website/blog generator. It is "Simple, Static, and Blog-aware". To get started, you can [Jekyll Now](http://www.Jekyllnow.com/) without too much programming knowledge, or you can [Build A Blog With Jekyll And GitHub Pages](http://www.smashingmagazine.com/2014/08/build-blog-Jekyll-github-pages/)
 if you are willing to do some digging and stuff. I benefits a lot from these resources and I'm going to tell you how I build my Jekyll blog.

## 1. Install Jekyll

Jekyll is written in [Ruby](https://www.ruby-lang.org/en/), so you have to install Ruby first. Mac has Ruby installed by default, but I don't want to mess up with the default system installation. A better way is to install a different version of Ruby by yourself.

### install homebrew

And to "install the stuff you need that Apple didn’t", it's better to use [homebrew](http://brew.sh/). To install homebrew, run this script:

~~~
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
~~~

### install rbenv

Then, I install [rbenv](https://github.com/rbenv/rbenv) using homebrew. It is used to manage different version of Ruby.
</br>
About how it works: "rbenv intercepts Ruby commands using shim executables injected into your PATH, determines which Ruby version has been specified by your application, and passes your commands along to the correct Ruby installation."

It's super eazy to install it with homebrew in Mac, just run the following commands:

~~~
$ brew update
$ brew install rbenv ruby-build
~~~

### install ruby with rbenv

Now it's time to install ruby:

~~~
# list all available versions:
$ rbenv install -l

# install a Ruby version:
$ rbenv install 2.0.0-p247
~~~

### install Jekyll

Finally, I install Jekyll.

~~~
$ gem install Jekyll
~~~

Done! You can run Jekyll and build a static website!

## 2. Write a new blog

Start your file with [Front Matter](http://Jekyllrb.com/docs/frontmatter/) like this:

~~~
---
layout: default
title: My First Blog
---
~~~

Then write your blog in the format of [markdown](https://daringfireball.net/projects/markdown/) like this:

~~~
# Title

## First
asdasdasdasdasd

## Second
asdasdasdasdasd

## Third
asdasdasdasdasd
~~~

Name it as `yyyy-mm-dd-post-title-example.md`.

## 3. Host the blog in local

Open cmd, type:

~~~
$ Jekyll new myblog
~~~

It will create a new blog named "myblog" in the current location. The directory of the blog system looks like this:

~~~
.
├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _data
|   └── members.yml
├── _site
├── .Jekyll-metadata
└── index.html
~~~

Put your blog under the `_posts` folder.

Then serve the blog using:

~~~
$ Jekyll serve
~~~

You can access the blog in http://localhost:4000

## 4. Host the blog in github

Of course you want to put your blog site online. One way is to buy a domain and hosting server, another way is to host with github-pages. To do this, create a new project in github, upload your entire local blog folder to github. (except `.site` which is a temp folder for rendering the site, github-pages will generate this folder by itself)

And you should be able to access the blog in the url of "https://github.com/username/project-name".

### optional: install github-pages

Sometimes you will see your local blog slightly different from github-pages, it's mostly because you are using a different version of ruby & Jekyll from [github-pages]((https://pages.github.com/)). To sync these two, you can install a `github-pages` gem and use it to serve Jekyll in local. What you need to do is:

- install ruby-build-github with rbenv
- install github-pages
