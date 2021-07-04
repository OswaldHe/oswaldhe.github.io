---
title: "All Blogs"
permalink: /blogs/
layout: archive
author_profile: true
---

<p>
Here is a collections of my software dev notes and working/researching experience (Maybe also some hobbist chats). Feel free to leave comments!
</p>

<ul>
    {% for post in site.posts %}
    {% unless post.next %}
      <h2 style="color: #bdc1c7">{{ post.date | date: '%Y %b' }}</h2>
    {% else %}
      {% capture ym %}{{ post.date | date: '%Y %b' }}{% endcapture %}
      {% capture nextym %}{{ post.next.date | date: '%Y %b' }}{% endcapture %}
      {% if ym != nextym %}
        <h2 style="color: #bdc1c7">{{ post.date | date: '%Y %b' }}</h2>
      {% endif %}

    {% endunless %}
   {% include archive-single.html %}
  {% endfor %}
</ul>