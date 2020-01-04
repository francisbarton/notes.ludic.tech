---
title: Making a tag cloud with eleventy
description: working notes on an adventure in JavaScript
tags:
  - eleventy
  - notes
  - learning
layout: layouts/post.njk
---

Here's the story of me trying to do something with eleventy, and what I learned along the way.

There are a couple of things I would like to do to tinker with base eleventy: a tag cloud and markdown snippet processing. I'll just stick to the tag cloud here, as I got [really stuck](github issue links) trying to do the markdown thing and it looks like someone else may amazingly have [solved that one anyway](https://www.npmjs.com/package/eleventy-plugin-markdown-shortcode). (But I haven't yet tested if that plugin does what I hope it does.)

## Laying out the challenge
So I was looking at the `tags-list.njk` bit of eleventy, which looks like this:

```liquid
{%- raw -%}
---
permalink: /tags/
layout: layouts/home.njk
---
<h1>Tags</h1>

{% for tag in collections.tagList %}
	{% set tagUrl %}/tags/{{ tag }}/{% endset %}
	<a href="{{ tagUrl | url }}" class="tag tags_page size{{ tagNum }}">
	{{ tag }}</a>
{% endfor %}
{%- endraw -%}
```

and makes the [tag list page](/tags/). And I thought to myself, "Hmm, if we knew how many posts were tagged with each tag, you could make the more popular tags bigger, like a tag cloud."

My idea was that, just as you have a `tagUrl` being set in the template above for each tag, you could have a `tagPop` being set up too, based on how popular each tag is. And then this would get included as part of a CSS class, and the CSS file would then use `--calc()` to set the font size of the tag on the page.

I haven't really thought this through. But it's an idea.

Then I started looking through `.eleventy.js` to find out how I could make this work. I then ended up look
