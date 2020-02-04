---
title: getting fresh with a brand new project
description: 
date: 2020-01-23
tags:
  - notes
  - #twirl
layout: layouts/post.njk
---

Today I started off a brand new project that I will do mostly in R. The project is researching digital exclusion risk patterns in [Ashfield district][1], Nottinghamshire.


The [last time][2] I started a project like this at work, I was really using R for the first time 'in anger' and though I had learned some things already, there was a huge amount I still had to learn about how to structure an R script and how to work with data efficiently, let alone how to put together a project that works with multiple files and datasets and functions. I ended up making a massively long and inefficient .Rmd file full of repetition.

I know that I still have a lot more to learn - partly as I am still in my first year of working with R and partly because there are always new developments and exciting new packages to work with.

Since then I have done bits of analysis and mapping as part of various projects, learning lots about functional programming, writing functions, writing more economical code and about how to structure projects (both in terms of directory structure and in terms of workflow).

So now I have the opportunity to put some of that learning into practice with this project and do a much better job of getting things well structured right from the start - if not perfect then at least in a state where tweaks and changes can be made without a full-on project structure.

Some of the things I am aiming to implement here, in very abstract terms (I may add specific links later) are:
- [workflowr][3] folder setup
- reproducible code i.e. scripts that contain all the steps necessary to run and re-run the analysis from the start such that all necessary information and software is present
- regular git commits for full versioning history
- iterative workflow and working in the open (rapid prototyping) i.e. my work will be regularly output to an html file that will be a snapshot of latest project status
- "[RMarkdown Driven Development][4]", or something like it
- making an R package (I doubt I will do this; but it, or parts of it, might end up suiting a package)
- journalling my learning - which is what this post is! - my thoughts and frustrations and revelations

[1]: [https://en.m.wikipedia.org/wiki/Ashfield_District]
[2]: [https://citizens-online.github.io/epping-forest/]
[3]: [https://jdblischak.github.io/workflowr/]
[4]: [https://emilyriederer.netlify.com/post/rmarkdown-driven-development/]
https://emilyriederer.netlify.com/post/resource-round-up-reproducible-research-edition/