---
layout: post
title: How I got Jekyll to work the way I want
date: 2020-03-17 19:48 -0600
category: Web
tags: [ruby, jekyll]
---

Okay, I've been *(seriously this time)* playing around with Jekyll and
I think I can now give a writeup on how I got this site to a point
I feel confortable with.

I know that the Jekyll experience is a little different when using 
GitHub Pages vs generating you own static content and then pushing it
to `master`, but since I'm not a fan of front-end stuff (CSS is a
nightmare for me), I knew from the beginning that if I wanted to make
this site, it had to be the easiest thing for me to maintain.

So yes, I went with [GitHub Pages](https://pages.github.com/), but
unfortunately, GitHub Pages generates all pages using the `--safe`
options, which means that third-party plugins are out of question
*(GitHub has a whitelist of plugins you can use that can be found
[here](https://pages.github.com/versions/))*. Due to the lack of 
plugins, you have to be a little handy to get where I wanted to get.

<!--more-->

What do I want this site to have? I want it to be as clean and simple
to navigate as possible, I want it to be more than a blog (so having
a sidebar was a must), have some sort of comment system, have a *"Read
more"* link (so you skip the wall of text you find uninteresting) and
have a search function.

### Theme and layout

For the theme, I just used the [Hyde Theme](https://github.com/poole/hyde)
because it's pretty much what I wanted and it was easy to install. The only
tweaks I made to it was adding the avatar on the sidebar (I still need to
fix the centering on desktop *sigh*) and replacing the copyright notice
with the Creative Commons notice (Free licences ftw).

### Comments section

I want to have a comments section under every post, so if anyone wants
to add something, or correct me if I'm wrong, that person
can do it easily and in a visible way. As I said before, I wanted the 
easiest thing I could find, also I cannot install plugins, so I
went with [Disqus](https://disqus.com/).

#### How to install Disqus on Jekyll

The installation is pretty straight-forward. Disqus provides something
they call a [Universal Embed Code](https://help.disqus.com/en/articles/1717112-universal-embed-code),
which is just an HTML/JavaScript snippet containing the comment box for
your site.

What you have to do is add the following snippet to `_layouts/post.html` where you want
you comment box to be:

```html
{% raw %}
{% if page.comments %}

<h2>Comments</h2>
<!-- YOUR DISQUS UNIVERSAL EMBED CODE HERE -->
{% endif %}
{% endraw %}
```

You can customize the block as much as you like, of course. After that,
just add `comments: true` to the [YAML Front Matter](https://jekyllrb.com/docs/front-matter/)
of the posts/pages for which you want to enable comments.

### Showing a teaser and adding a "Read more" link

I want to show just an excerpt of every post on the index page,
in order to avoid getting too much clutter on the main page. If the visitor wants to
keep reading, just press the "Read more" link.

I got this working following [Purpzie's](https://github.com/Purpzie)
[super helpful answer](https://github.com/mmistakes/jekyll-theme-hpstr/issues/194#issuecomment-388390761).

Just replace `{% raw %}{{ post.content }} {% endraw %}` on your `index.html` with the following:
```html
{% raw %}
{{ post.excerpt }}
{% capture content_words %}{{ post.content | number_of_words }}{% endcapture %} 
{% capture excerpt_words %}{{ post.excerpt | number_of_words }}{% endcapture %} 
{% if excerpt_words != content_words %}
<a href="{{ post.url }}#read-more" class="btn btn-info">Read More</a>
{% endif %}
{% endraw %}
```

Then I just added a more explicit excerpt separator to indicate
where I want to end the excerpt. Just add the following to your
`_config.yml`:
```yaml
excerpt_separator: "<!--more-->"
```

Then I just add a line on my post with `<!--more-->` where I want the excerpt to end.

Finally, I want a search function, which I have yet to implement
because I haven't found a solution simple enough for this lazy
individual to integrate. I will write a post when I get one working.

That's the post for today! Hope it's useful :)
