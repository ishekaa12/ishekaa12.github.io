---
layout: home
title: Home
---

# Isheka's Blog

Thoughts on college, code and life.

{% raw %}
{% for post in site.posts %}

## [{{ post.title }}]({{ post.url }})

{{ post.date | date: "%B %d, %Y" }}

{{ post.excerpt }}

---

{% endfor %}
{% endraw %}
