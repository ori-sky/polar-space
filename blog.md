---
title: Stuff What We Wrote
---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | remove_first:'/' }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ2NzA3ODkxNV19
-->
