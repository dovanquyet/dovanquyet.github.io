---
permalink: /blog/
layout: archive
title: "Blog"
author_profile: True
redirect_from:
  - /blog-posts/
---

View posts by [topics](https://dovanquyet.github.io/categories/). Some interesting topics are: infographic, philosophy, ...
All topics (by alphabetical order) are book review, cool, funny, infographic, occasion, philosophy, work and research. To understand me, please read posts of the topic "my character".

Xem các bài viết theo chủ đề tại [đây](https://dovanquyet.github.io/categories/). Một số chủ đề hay ho đó là infographic (tin dạng ảnh), philosophy (triết học).

{% include base_path %}
{% capture written_year %}'None'{% endcapture %}
{% for post in site.posts %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% if year != written_year %}
    {% capture written_year %}{{ year }}{% endcapture %}
  {% endif %}
  {% include archive-single.html %}
{% endfor %}