---
layout: post
title: relative URLs in Jekyll sites
---

The most frustrating thing about trying to put a Jekyll site on GitHub Pages is that by default, it assumes that the base URL is `<username>.github.io` and generates all the other URLs relative to that. It always takes me a good 15 minutes to figure out what's going on and why the site won't show up at the expected URL.

For future reference: all you have to do is set the `baseurl` parameter in `_config.yml` to the relative path of your repo (in this case `/notes`) and it'll [work](http://jekyllrb.com/docs/github-pages/#project-page-url-structure) as long as you've got the `relative_url` filter added to every URL on the site.