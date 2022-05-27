---
layout: post
title:  Hello, World!
date:   2022-05-26 00:00:00
description: Caveats about GitHub Pages, Jekyll and al-folio theme.
tags:
categories:
comments: true
---



As I'm about to start a new chapter in life, it's a good opportunity create a new homepage. I found [al-folio](https://github.com/alshedivat/al-folio) to be an excellent choice. It's a [Jekyll](https://jekyllrb.com/) theme designed for academics, and can be hosted using [GitHub Pages](https://pages.github.com/).



Just like most modern web technologies, it's a bit convoluted at first glance. Here is a TL;DR description about how it works: The actual website are static files, stored in the `_site` directory. They are compiled from configuration, markdown and template files using `jekyll build` command. When using [al-folio](https://github.com/alshedivat/al-folio), the `_site` directory on your main branch is empty and ignored by git. Every time you push modifications to GitHub, a [GitHub Action](https://github.com/features/actions) is triggered. GitHub's servers will then compile your website and save it to another branch called `gh-pages`, which is used by GitHub to host your website.



Setting it up is relatively straightforward, just follow the step-by-step instructions in [al-folio](https://github.com/alshedivat/al-folio)'s `README.md`. But there are some caveats it did not cover.



### Caveat 1 - Custom Domain Name and HTTPS

I encountered this error message when trying to enable HTTPS.

```
Certificate Request Error: Certificate provisioning will retry automatically in a short period, please be patient.
```

In my specific case, the solution is very counterintuitive. GitHub won't issue the TLS certificate unless the `www` CNAME record of your domain name points to `your-username.github.io`.  See this [answer](https://github.community/t/certificate-request-error-is-persistent-tls-certificate-cant-be-provisioned/11008/17).



### Caveat 2 - macOS and Ruby

When using [al-folio](https://github.com/alshedivat/al-folio), some dependencies won't compile if you are using the builtin ruby on macOS. The easiest fix I found is to install ruby through `brew`, then add

```shell
export PATH="/usr/local/opt/ruby/bin:$PATH"
```

to your rc files. This will make the shell choose the ruby from `brew` over the system default. Technically, there are more elegant solutions using the `virtualenv` equivalent for ruby. But let's be honest, we don't want to learn ruby. :man_shrugging:



### Caveat 3 - Custom Domain Name and CNAME File

At this point, every time you push changes to GitHub, your settings for custom domain name will be gone (you will get a 404 error). To fix this, you should add a file on your main branch named `CNAME` containing the domain name you want to use.



The reason is, when you set custom domain name through the GitHub menu, what it actually does is add a file named `CNAME` containing that domain name, on the `gh-pages` branch. Every time you push changes to GitHub, the `gh-pages` branch is purged and recreated, so is the `CNAME` file in it.



Just to be safe, I also set up [a free domain monitor](https://www.checklyhq.com/). It will send me an email when the website is down.



---



Other than these caveats, I really enjoyed this setup. Highly recommend [al-folio](https://github.com/alshedivat/al-folio) for making your personal homepage.