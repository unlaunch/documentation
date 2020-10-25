---
title: FAQ
permalink: /faq/
---

# Frequently Asked Questions
<br>

{% for post in site.posts limit:100 %}
   <div class="post-preview">
   <h3><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h3>
   {{ post.content | split:'<!--more-->' | first }}
   {% if post.content contains '<!--more-->' %}
      <a href="{{ site.baseurl }}{{ post.url }}">read more</a>
   {% endif %}
   </div>
   <hr>
{% endfor %}

**Do you have a question?** Please <a href="{{ site.repo }}/issues/new?labels={% if page.editable %}{{ page.editable }}{% else %}question{% endif %}&title=Question:&body=I have a question." target="_blank">ask it here <i class="fab fa-github fa-fw"></i> </a>

