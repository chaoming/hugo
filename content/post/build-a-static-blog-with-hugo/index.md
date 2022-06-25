+++
author = "Chaoming Li"
title = "Build a Static Blog with Hugo"
description = "I rebuilt my old personal blog with Hugo, a popular static website builder, and hosted it on Github pages. This is why and how I did it."
date = "2022-06-25"
categories = [
    "DevOp"
]
tags = [
    "github",
    "hugo",
    "static website",
    "firebase",
    "notion",
    "markdown"
]
image = "hugo.png"
+++

## Why Static Website Builder

I had a blog that was using WordPress and it was hosted on an AWS EC2 + CloudFront CDN. It was fast and not too hard to use, except that sometimes it might break down because of a WordPress update or issues with the MySQL database. Since I built the [fireact.dev](http://fireact.dev) website with Jekyll, I have been thinking to move the old blog site to a static solution because:

- No need for a database, which means less maintenance headache
- Free hosting options like Github and Firebase
- Fast, super fast! That is good for UX and SEO

## The Solution

I did some research and decided not to use Jekyll because I found [Hugo](https://gohugo.io/) is gaining popularity and it has a lot of free themes too. I picked a theme called [Stack](https://themes.gohugo.io/themes/hugo-theme-stack/), which looks great in my opinion and is well maintained by its creator.

Creating a Hugo site is quite simple following its step-by-step instruction document. The themes are installed as `git submodules` which makes them relatively simple to install.

Hugo also has a deployment config for Github pages so you can deploy your website by pushing commits to a branch.

## Writing Posts with Markdown

The most common way to write content for static websites is to create markdown files. Markdown is widely used in writing documentation for tech projects. You don’t need to worry about HTML tags, just write with some formatting syntax like “##” means <h2> tag in HTML.

If you don’t want to write with markdown syntax, [Notion](https://www.notion.so/) is a great tool. It has a very user-friendly interface for writing content. Once you finish an article, select the whole article and copy it, and you can paste content to a markdown file.

## My Thoughts

There are many open-source projects are using static website builders for building websites for the projects and are hosting the websites on Github.

I think it’s totally possible to use the same solution for early-stage startups to build their websites and host them on Firebase. For SaaS startup, my thinking is that a static website for the content, and [fireact.dev](http://fireact.dev) for the web application. All these can be hosted on Firebase with minimum costs.

Although Notion is a great tool for writing and converting content to markdown format, I think there is a need for some kind of CMS application that can do a similar job and automate the process to update and deploy static websites. Or maybe just automate the deployment process of Notion articles to static websites, because Notion is already doing a really good job as a writing tool. So that people who don’t know much about Git can also easily update their static websites.