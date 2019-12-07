# notes-ludic-tech

My version of the [Eleventy](https://github.com/11ty/eleventy) static site generator, using [eleventy-base-blog](https://github.com/11ty/eleventy-base-blog). This repo uses netlify to make my [experimental notes site](https://notes.ludic.tech).

[![Build Status](https://travis-ci.org/11ty/eleventy-base-blog.svg?branch=master)](https://travis-ci.org/11ty/eleventy-base-blog)

### Saturday 2019-12-07
This is, like, the fifth time I have tried to make this blog with eleventy. Something went wrong with the last go and I made it worse by trying to fix it, and I didn't do sufficiently regular git commits to do a revert I was happy with, so here I am like an idiot trying again.

### Sunday 2019-11-24
This is, like, the fourth time I have tried to make this blog with eleventy, and every time so far I screw things up by trying to make it do clever things. That's creativity for you :-/

I really like eleventy so far, except I don't have the knowledge of JavaScript to really understand how things work under the hood. Hopefully by tinkering, I will learn as I go, as well as making the site my own, not just the vanilla base blog.

So this time, my intention is to use `git` properly, doing much more regular commits and making it so that I can revert to a functional site if^H^Hwhen I break something. Also it's good for documenting what I am trying to do.

### Implementation Notes

* `about/index.md` shows how to add a content page.
* `posts/` has the blog posts but really they can live in any directory. They need only the `post` tag to be added to this collection.
* Add the `nav` tag to add a template to the top level site navigation. For example, this is in use on `index.njk` and `about/index.md`.
* Content can be any template format (blog posts needn’t be markdown, for example). Configure your supported templates in `.eleventy.js` -> `templateFormats`.
	* Because `css` and `png` are listed in `templateFormats` but are not supported template types, any files with these extensions will be copied without modification to the output (while keeping the same directory structure).
* The blog post feed template is in `feed/feed.njk`. This is also a good example of using a global data files in that it uses `_data/metadata.json`.
* This example uses three layouts:
  * `_includes/layouts/base.njk`: the top level HTML structure
  * `_includes/layouts/home.njk`: the home page template (wrapped into `base.njk`)
  * `_includes/layouts/post.njk`: the blog post template (wrapped into `base.njk`)
* `_includes/postlist.njk` is a Nunjucks include and is a reusable component used to display a list of all the posts. `index.njk` has an example of how to use it.
