---
title: 'Website Update: Jekyll'
categories: ["Projects"]
tags: ["design", "web", "html", "static-site", "jekyll", "github"]
description: "Switching from ProcessWire to Jekyll as back-end that runs this website."
cover: /images/cover.jpg
date: 2016-07-26 00:00:00
---

## Background

I get bored, but I really love learning. This combination leads me to regularly experiment with new technology and tools to see how they stack up with what I am currently using. My personal website, [nathanpetersen.com](http://nathanpetersen.com) tends to be the guinea pig for such experiments.

When I first obtained the nathanpetersen.com domain, I wasn't sure what I wanted to do with it, so I resisted the temptation to install a CMS (i.e., Drupal, Wordpress, ProcessWire, etc.). For a while, I used the site to host little projects for school or play, but the site just wasn't being used for anything productive. There was no reason anyone would ever want to come to it, besides me. Moreover, there was no index of the projects on it -- just type in the magic path. This worked for me, but I wanted better.

The first goal was to create a website on which I could publish writeups about projects I made or musings from my head about life. On top of that, I wanted an environment where I could continue to hack around and make little projects. So basically, I wanted a blog with a section for freeform pages.

Enter [ProcessWire](http://processwire.com).

## ProcessWire

I have used ProcessWire for a couple other projects, with the most prominent being [team4096.org](http://team4096.org). I have dabbled with Drupal, but all my experiences with it have been really bad. There is a definitely a sour taste is my mouth from Drupal. ProcessWire was designed to be more developer friendly. I really like the design of it, plus it could satisfy my requirements for the site.

I installed PW and built a simple blog on top of it. Not too bad -- it seemed to work just how I wanted. Everything was going fine until I got bored.

While nothing was really _wrong_ with ProcessWire, it just seemed like a rather heavy solution for a simple blogging site with a couple extra custom pages. Page load times weren't exactly snappy and it required a lot of database queries and server side code just to spit out a simple text document. Why use a solution that wasn't optimal? 

On top of all of that, paying for hosting a blog seemed silly. There are a lot of solutions out there that hosts content without any charges (Medium, Tumblr, GitHub Pages, etc). As I looked into different tools and technologies, I really started looking into Amazon Web Services for hosting. I setup a simple LAMP stack on a free EC2 instance and installed my ProcessWire blog setup onto it. It worked, but it got me thinking.

**Why pay to run an EC2 instance continuously if the content rarely changes (and probably isn't accessed that much)?**

Enter [Jekyll](https://jekyllrb.com/).

## Jekyll

What exactly is Jekyll? A CMS? A publishing platform? According to Jekyll's website:

> Jekyll is a simple, blog-aware, static site generator. It takes a template directory containing raw text files in various formats, runs it through a converter (like [Markdown](https://daringfireball.net/projects/markdown/)) and our [Liquid](https://github.com/Shopify/liquid/wiki) renderer, and spits out a complete, ready-to-publish static website suitable for serving with your favorite web server. Jekyll also happens to be the engine behind [GitHub Pages](https://pages.github.com/), which means you can use Jekyll to host your project’s page, blog, or website from GitHub’s servers **for free**.

Wow! Free, easy, no hosting, static, GitHub, Markdown -- I'm ready to try it!

I rebuilt the same UI from my ProcessWire blog using Liquid and converted all my projects into Markdown for Jekyll to use. This wasn't super easy, but since I only had a few posts it wasn't that bad. Plus, a few online tools helped the process: [converting whole pages to Markdown](http://fuckyeahmarkdown.com/), [easily making Markdown tables](http://www.tablesgenerator.com/markdown_tables).

## Old vs. New

Using Jekyll eliminates the hosting concern by using GitHub Pages. It also makes it trivial to update and maintain version control of the blog. GitHub's online interface can be used to update the website, plus it's easy as pie to make a new post from the command line.

```
cd github-project
vim _posts/yyyy-mm-dd-post-title.markdown
git add .
git commit -m "new post"
git push
```

The only other concern was page load time. Hopefully, serving static HTML files from GitHub's CDN would be faster than PHP rendered HTML from some random shared hosting server. Time to see if it actually is faster with a quick comparison.

I ran both the ProcessWire site and the Jekyll powered site through [Pingdom Tools](https://tools.pingdom.com/) and gathered the following load times:

| Page         | ProcessWire | Jekyll | Improvement |
|--------------|-------------|--------|-------------|
| /            | 1.68 s      | 222 ms | 7.6 x       |
| /contact/    | 488 ms      | 243 ms | 2.0 x       |
| BCLD project | 1.46 s      | 440 ms | 3.3 x       |

The `/` page changed slightly, but isn't really a fair comparison between the two systems. The `/contact/` page is a great comparison --  a completely static page that was loaded twice as fast in the new system. A project example, the BCLD project page, improved 3.3x!

While my tests weren't exactly scientific, I think this shows that using Jekyll and GitHub Pages definitely speeds up page load time for nathanpetersen.com.

## Conclusion

Updating my personal blog to use Jekyll and GitHub Pages was an easy transition that definitely felt refreshing. While I do like server-side coding and it absolutely has it's place, it feels good to get rid of it from my workflow and worry solely about content creation and site styling.

Give Jekyll a try the next time you have a simple project that fits its demographic. Your time and energy invested in it will go a long ways.