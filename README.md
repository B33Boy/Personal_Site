[![Netlify Status](https://api.netlify.com/api/v1/badges/c02584e1-dda3-4c5a-a453-b1ba572cd3b7/deploy-status)](https://app.netlify.com/sites/abhipatel/deploys)

### How to Add Categories for Blog posts
Create a new markdown file in `categories/` \
Replace `CATEGORY_NAME` and `CATEGORY_TITLE` and enter the following

```
---
layout: page
title: CATEGORY_TITLE
permalink: /blog/categories/CATEGORY_NAME
---

<h5> Posts by Category : {{ page.title }} </h5>

<div class="card">
{% for post in site.categories.CATEGORY_NAME %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>
```

### How to update Algolia Indices
`bundle exec jekyll algolia`
