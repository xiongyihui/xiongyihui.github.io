---
layout: page
title: Welcome!
tagline: here meet cats
---
{% include JB/setup %}

## Never limit your imagination!

![Tangram Cats](/assets/images/tangram_cat.png)

## Latest blog
<ul>
    {% for post in site.posts limit 4 %}
        <li>
            <span>{{ post.date | date_to_string }}</span> &raquo;
            <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
        </li>
        {{ post.content | strip_html | truncatewords:32}}<br>
        <a href="{{ post.url }}">Read more...</a><br><br>
    {% endfor %}
</ul>



