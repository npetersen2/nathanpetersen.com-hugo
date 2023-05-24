# nathanpetersen.com-hugo

> Nathan's blog website, built using the hugo static site framework.

## Background

Over the years, NathanPetersen.com has been built using several different web technologies. In May 2022, Nathan rebuilt the site, once again, this time using `hugo`, a very popular static site generator built on top of the `go` language. It took about 20 hours of work time for the rewrite.

There is NO `go` code involved in the site! Simply the compiler which takes the markdown and template source and converts to HTML is written in `go`. Nathan wrote zero `go` code during the rewrite! However, he did create multiple `shortcode` snippets which effectively implement widgets in the website; these are built using the `hugo` templating framework.

## Development

Nathan implemented the following to rewrite the site:

1. Install WSL 1 (?) on Windows 10, running Ubuntu 20 (as of May 2022, older Ubuntu versions had too old `hugo` packages)

2. Use `apt-get` to install the `hugo` package
> `sudo apt install hugo`

3. Confirm the version of `hugo`:
> `hugo version`
>
> `Hugo Static Site Generator v0.68.3/extended linux/amd64 BuildDate: 2020-03-25T06:15:45Z`

4. Clone GitHub repo for website to WSL and open code in VSCode by running `code` at prompt

5. Turn on live local server:
> `hugo serve --disableFastRender`

6. Edit website

7. Look at results by loading [http://localhost:1313/](http://localhost:1313/) in browser

8. When done, build `public/` output of HTML files by simply running:
> `hugo`

## HTML Check

Unfortunately, the generated HTML is not validated for things like dead links, etc.

You can do this by using the nice `htmltest` tool (https://github.com/wjdp/htmltest):

1. Build the HTML output using `hugo`

2. Download the `htmltest` tool (puts it in the `./bin` folder)
> `curl https://htmltest.wjdp.uk | bash`

3. Delete the `tmp/` folder output (if you already ran `htmltest`)

3. Run the tool:
> `./bin/htmltest -c .htmltest.yml`


## Autobuild

The GitHub repo is set up to automatically rebuild and publish the website on push to the `main` branch. The rebuild process takes several minutes due to installing `hugo`.

The website it published from the `gh-pages` branch and is served from the `docs/` folder.
