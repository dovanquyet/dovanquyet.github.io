---
permalink: /blog/
layout: archive
title: "Blog"
author_profile: False
redirect_from:
  - /blog-posts/
---

View posts by [topics](https://dovanquyet.github.io/tags/). Some interesting topics are: infographic, philosophy, ...
All topics (by alphabetical order) are cool, funny, infographic, occasion, philosophy, work and research.

{% include base_path %}
{% capture written_year %}'None'{% endcapture %}
{% for post in site.posts %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% if year != written_year %}
    {% capture written_year %}{{ year }}{% endcapture %}
  {% endif %}
  {% include archive-single.html %}
{% endfor %}
