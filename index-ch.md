---
layout: home
author_profile: true
lang: zh
ref: index
header:
  overlay_color: "#000"
  overlay_filter: "0.2"
  overlay_image: /assets/images/beach-cropped.jpg
  cta_label: "访问我的github"
  cta_url: "https://github.com/alexQiSong"
excerpt: "从数据之海中挖掘知识"
---
<ul>
{% assign posts=site.posts | where:"ref", page.ref | sort: 'lang' %}
{% for post in posts %}
  <li>
    <a href="{{ post.url }}" class="{{ post.lang }}">{{ post.lang }}</a>
  </li>
{% endfor %}

{% assign pages=site.pages | where:"ref", page.ref | sort: 'lang' %}
{% for page in pages %}
  <li>
    <a href="{{ page.url }}" class="{{ page.lang }}">{{ page.lang }}</a>
  </li>
{% endfor %}
</ul>

