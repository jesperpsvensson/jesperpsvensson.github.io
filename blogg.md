---
layout: default
title: Blogg
permalink: /blogg/
---
<h3 class="marginTop title">Inlägg</h3>
<div class="border"></div>
<ul class="bigMarginTop" style="list-style:none; padding:0;">
  {% for post in site.posts %}
    <li style="padding-top: 15px;">
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
