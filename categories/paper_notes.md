---
layout: page
title: Paper Notes
permalink: /blog/categories/paper_notes
---

<h5> Posts by Category : {{ page.title }} </h5>

<div class="card">
{% for post in site.categories.paper_notes %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>
