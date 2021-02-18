---
layout: post
title: Simple Jekyll Search Page
date: 2020-07-14 20:16 -0600
category: web
tags: ruby jekyll
---

The last time I wrote on this blog (and it was a long time ago, shame on me), I discussed the different functionalities I wanted to have on my blog, one of them being [a search function easy enough to install](https://andres.world/2020/03/18/jekyll-first-review/), and until today, I haven't found a suitable solution.

I was not until this morning that I ran into a [simple script](https://jekyllcodex.org/without-plugin/search-lunr/) using [Lunr.js](https://lunrjs.com/). It's perfect: it's simple, it works, no database, no backend, and best of all, no Google trackers on my site.

Here's a write-up on how I installed it, but what I did it's no different to what's described on JekyllCodex.

<!--more-->

## Create the search box

First, I created the simple search box you can find on the [Search](http://andres.world/Search/) section on my site. I just created a new file named `search-box.html` under the `_includes` directory of my Jekyll project. [Here's the snippet](https://raw.githubusercontent.com/jhvanderschee/jekyllcodex/gh-pages/_includes/search-lunr.html).

## Install Lunr.js

Create a `js` directory on your Jekyll project (if you don't already have one) and create a new file named `lunr.js` inside it. Paste the contents of [this snippet](https://raw.githubusercontent.com/jhvanderschee/jekyllcodex/gh-pages/js/lunr.js).

## Create the search page

Finally, using [Jekyll Compose](https://github.com/jekyll/jekyll-compose), I create a new page:
```bash
bundle exec jekyll page Search
```

And embed the newly created search box:
```html
{% raw %}
---
layout: page
title: Search this site
---

{% include search-box.html %}
{% endraw %}
```

And that's it! It's pretty, responsive, and won't track the user :)

## About DISQUS

I know I recommended using DISQUS as a way to simply add a comment section to your website, however I was never confortable with that solution. First, it's SLOOOW, and the main reason I use Jekyll is having a minimal website that loads fast.

Second, and the most important one, it adds a lot of trackers to my website that I rather not have (If you value internet privacy as much as I do, you'll understand).

So after much consideration, I decided to completely remove the comment section on my blog, however that does not mean I'm not open to discussion, so if you want to comment a post with me, feel free to [reach me](http://andres.world/about/).

Maybe one day I'll find a solution for the comment section without all those pesky trackers, but as of today, I have no comment section, but also, as displayed by the awesome [Privacy Badger extension](https://privacybadger.org/) developed by our good friends, the [Electronic Frontier Foundation](https://www.eff.org/), my website is now **100% tracker-free** :)

Until next time!
