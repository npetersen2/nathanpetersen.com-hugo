---
title: 'Website Update: Hugo'
categories: ["Experiences"]
tags: ["design", "web", "html", "static-site", "hugo", "github"]
description: "Complete rewrite of my blog using Hugo for the static site generator back-end."
cover: /images/cover.jpg
date: 2022-05-25 00:00:00
math:
    enable: true
---

**Welcome back to my blog, now completely overhauled and rewritten using the fabulously fast [Hugo](https://gohugo.io/) static site generator framework!**

After a nearly 18 month hiatus from this blog, I finally dusted off the cobwebs. Or, more accurately, I threw away the old dusty blog and resurrected it using modern technologies. This post outlines what I did, why I did it, and what's next.

{{< rawhtml >}}<div style="margin-top:2em;">{{< /rawhtml >}}
{{< figure
    src="images/hugo-logo-wide.svg"
    caption="Hugo static site generator logo"
    linked=false
    class="image"
>}}
{{< rawhtml >}}</div>{{< /rawhtml >}}

## What Have I Been Doing?

From the total silence on this blog, it looks like I haven't been doing anything lately... However, that is far from reality.

It turns out that being a PhD grad student simply *eats* your time, sucking away days, nights, and weekends. If you're not careful to carve out chunks of time for personal projects, there is simply no room to do anything outside of school and research.

For the past 2.5 years, I have been a full-time grad student at the [University of Wisconsin–Madison](https://wisc.edu/) pursuing my MS and now PhD degrees in Electrical and Computer Engineering. The amount of knowledge I have gained in grad school is crazy; simply reflecting on my progression over just one year is incredible. For my M.S. research project, I created the Advanced Motor Drive Controller (AMDC) which is a collection of open-source hardware and software for controlling advanced motor drives. Check it out at [amdc.dev](https://amdc.dev/) or read the docs at [docs.amdc.dev](https://docs.amdc.dev/). Also, check out my research [publications](https://scholar.google.com/citations?oan5hE4AAAAJ).

Perhaps equally important to the knowledge gained are the numerous relationships forged with other grad students. UW-Madison has a very diverse grad student population, and I have genuinely enjoyed learning about international cultures. Overall, I'd say I have been enjoying grad school! :grin:

{{< image
    src="images/nathan-lab.jpg"
    caption="Me in the lab working on my research projects."
>}}

## Blog Technology Progression

Back to the blog upgrade...

{{< mermaid >}}
flowchart LR
    id1[Blog, 2015] --> id2
    id2[Blog, 2016] --> id3
    id3[Blog, 2017] --> id4[fa:fa-star Blog, 2022]

    subgraph "PHP: ProcessWire"
    id1
    end

    subgraph "Ruby: Jekyll"
    id2
    end

    subgraph "JavaScript: Hexo"
    id3
    end

    subgraph "Go: Hugo"
    id4
    end

    click id1 href "https://processwire.com/" _blank
    click id2 href "https://jekyllrb.com/" _blank
    click id3 href "https://hexo.io/" _blank
    click id4 href "https://gohugo.io/" _blank

    style id1 fill:grey,stroke:#333,stroke-width:3px,color:white
    style id2 fill:grey,stroke:#333,stroke-width:3px,color:white
    style id3 fill:grey,stroke:#333,stroke-width:3px,color:white
    style id4 fill:red,stroke:#333,stroke-width:3px,color:white
{{< /mermaid >}}

I created this blog around the time I started college, back in 2015. Back then, [static site generators](https://staticsitegenerators.net/) were still somewhat "new" (or at least new to me) and most popular websites still ran using a CMS on a dynamic server. Of course, the web is based on serving quasi-static HTML pages, but with the advent of CMS technologies like [Wordpress](https://wordpress.com/) and [Drupal](https://www.drupal.org/), the ability to easily publish content on the web became more accessible.

I came to building my blog after building numerous [PHP](https://www.php.net/)-based websites during high-school. The "better" of these sites used [ProcessWire](https://processwire.com/) for their CMS which allows for very flexible front-end development. Check out my best one, which I built about a decade ago for my robotics team: [team4096.org](http://team4096.org/).


My first draft of this blog was built in the technology I knew at the time: PHP-based ProcessWire. I developed the general theme which lives on today, and wrote my first posts. However, I was very intrigued by these "static site generator" things...

GitHub was one of the first to really push easy-to-use static sites via their [GitHub Pages](https://pages.github.com/) system. This framework allows people to write content using the simple [markdown](https://github.github.com/gfm/) syntax, which is similar to HTML, but without the bloat. Then, on `git push` to GitHub, an automated back-end task builds and publishes the new content to the web. Easy, simple, and minimal configuration needed.

### Jekyll

GitHub's static site generator is called [Jekyll](https://jekyllrb.com/), is built using the [Ruby](https://www.ruby-lang.org/) programming language, and was released around 2008.

When I started with Jekyll, I wanted the same front-end flexibility as ProcessWire, but using a static site generator. The default GitHub-provided theming and build system simply did not cut it, so I forged my own theme and built the site locally to control the entire process. Then, I pushed the fully compiled HTML to the GitHub repo and configured the repo to serve onto my custom domain, nathanpetersen.com.

It worked well, see my [post](/2016/07/26/website-update-jekyll/). Most importantly, it was **free** :moneybag::money_with_wings:: I did not have to pay to keep a server online. Also, I did not have to manage the server, installing updates and security patches.

However, I had never used Ruby and did not want to learn it... It felt rather "old" and weird, and more modern languages were calling my name.

### Hexo

[Hexo](https://hexo.io/) was released around 2012 and provides the same functionality as Jeykll, but, it is built on top of [Node.js](https://nodejs.org/). This means it is built using JavaScript, a more modern and widely used programming language.

After about a year of using Jekyll, I took the plunge and rewrote the entire blog using Hexo. This mostly involved recreating my theme and customizations. I never made a post about this rewrite on the blog, but as of 2017, this blog has been powered by Hexo and JavaScript.

In my opinion, the coolest feature of the Hexo-based blog was the form on the [contact](/contact/) page. I wanted readers to be able to send me emails, but did not want to give away my personal email due to spam. However, since the blog was just a collection of static HTML pages, there was no way to have a back-end server process send emails to me dynamically. To solve this, I rigged up a custom [AWS Lambda](https://aws.amazon.com/lambda/) function which accepts form submissions and then sends me an email. This so-called "[serverless computing](https://aws.amazon.com/serverless/)" approach is ideal for small static sites since you pay pennies per lambda function execution, as opposed to hourly for a full server. Plus, you do not have to manage server package upgrades and security patches. Awesome!

The blog worked wonderfully for many years. I wrote post after post to document project after project. Multiple of my articles ([1](https://hackaday.com/2018/12/29/pov-tops-hobbyist-abilities/),[2](https://hackaday.com/2019/03/24/grabbing-the-thread-spinlocks-vs-mutexes/),[3](https://hackaday.com/2021/02/06/open-source-thermostat-wont-anger-your-landlord/)) have been picked up by [Hackaday](https://hackaday.com/) drawing thousands of people to my corner of the internet.

However, as it aged, things started breaking. Some Hexo package upgrades were not backwards compatible, so features starting to break. But, most detrimental to the blog's livelihood was my gradual decline in interest for JavaScript. During undergrad, I discovered my true love for embedded systems, hardware, and control theory. Gone were the days of wanting to be a web developer, and gone were the days of wanting to support a blog built using a JavaScript-based static site generator.

## Goals for Blog Rewrite

This brings us to present day, 2022. My journey in life continues, my skills evolve, and so do my ambitions for this blog.

In the past year, I have spent **considerable** time building out a comprehensive [documentation website](https://docs.amdc.dev/) for the [AMDC](https://amdc.dev) platform. I built this using [Sphinx](https://www.sphinx-doc.org/), which is a "Python documentation generator" that has evolved into a generic docs website creator. Most notably, it is the tech behind the [Python docs](https://docs.python.org/) and even the [Linux kernel docs](https://docs.kernel.org/). Sphinx is a static site generator aimed towards technical documentation. 

I loved using Sphinx to build the AMDC docs website. Reflecting back, I think it was so great because I did not have to worry about the web development piece of the pie---no longer do I want to spend countless hours tinkering with web development tools to build websites. When I want to publish things online, I want to solely focus on content creation. Let the framework and theme do the web dev stuff, and let me do the content stuff.

I wanted the same content writing experience for my blog. I wanted to use all the cool [reStructuredText](https://docutils.sourceforge.io/rst.html) features and extensions, like native $\LaTeX$ math support and [admonitions](https://docutils.sourceforge.io/docs/ref/rst/directives.html#admonitions). And, I wanted the flexibility to easily build my own theme, widgets, and features to aid in content writing (I still have a sweet spot for web dev :wink:). Finally, I wanted the content writing experience to be [WYSIWYG](https://en.wikipedia.org/wiki/WYSIWYG), but still in markdown.

Enter [Hugo](https://gohugo.io/).

## Hugo

[Hugo](https://gohugo.io/) is a static site generator framework, was released around 2013, and is built using the "new" and modern [Go](https://go.dev/) programming language. Its claim to fame is being *fast*, e.g., renders a page of the site in a few milliseconds. It has a built-in powerful theming framework which allows for very flexible page designs, and is easy-to-use.

Users do not need to know Go to create a theme or write content. In contrast, Hexo custom widgets are written in JavaScript, which tightly couples the theming to the builders native language.

The insanely fast build times and live reload local development server create an effectively WYSIWYG content creation experience: simply write markdown content in a text editor, save the file, and within half a second, the browser refreshes to show the changes. It's awesome.

Over the course of a few days, I spent 20 hours to do a complete rewrite of my blog using Hugo. I rewrote my blog's theme based off the popular [LoveIt](https://hugoloveit.com/) theme, and painstaking went through each and every post to update the markdown syntax for images and media. Overall, it went well and I love the results. It even has a dark mode now!

I also spent some time to rig up a [GitHub Actions](https://github.com/features/actions)-based website build script. Now, when I `git push` to the `main` branch, GitHub runs my action which builds the site with `hugo` and publishes the HTML files onto to web. All for free! Gone are the days of building locally and pushing the final HTML files up to GitHub. Frictionless content creation.

## Results

{{< mermaid >}}
flowchart TD
    id1("fa:fa-face-frown Uninspired") ==>|Hugo Static Site Generator| id2("fa:fa-face-smile Inspired")
    id2 --> id2

    style id1 fill:red,stroke:#333,stroke-width:3px,color:white
    style id2 fill:green,stroke:#333,stroke-width:3px,color:white
{{< /mermaid >}}

{{< rating 5 5>}}

After years of inspirational drought, I am truly thrilled to have my blog back in a state where I look forward to writing articles and creating content. The new extensions and features of the LoveIt theme along with my custom additions make writing content exciting again. Be on the lookout for more articles soon!

{{< admonition type=warning title="(Pseudo) Warning" open=true >}}
This is only the beginning of what's to come!
{{< /admonition >}}

{{< expand tag="h4" text="Do you dare expand me?" expanded=false >}}

You found me, :wink:.

Aren't these new widgets great!

{{< /expand >}}

**...and, $\LaTeX$ math** where $y = mx + b$ and $1+1 \ne 2$

:heart::heartbeat::heart:

$$
\sum_{k=1}^{\infty} \frac{1}{k^2} = \frac{\pi^2}{6}
$$

and you can copy the $\LaTeX$ commands straight from the browser by simply highlighting the equations and copying. Try it!

Or, what about code highlighting:

{{< highlight cpp >}}

void main() {
    printf("Hello World\n");
}

{{< /highlight >}}

This new blog framework is definitely not:

{{< rating 1 5>}}

At least:

{{< rating 4.5 5>}}

Thanks for reading!

---Nathan from Palo Alto, CA, USA

#### New Post Notifications

If you read this far, you might be interested in subscribing for email notifications about future new posts I write. I will never spam you! Every few months, I write a new article for this website, and will send you email about it. You can unsubscribe at any time. Thank you.

{{< mailchimpform >}}