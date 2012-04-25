---
layout: default
---

## Recent Posts
<section>
{% for page in site.posts limit:5 %}
<div class="post">
<h3><a href="{{ page.url }}">{{ page.title }}</a></h3>
{{ page.content }}
</div>
{% endfor %}
</section>

