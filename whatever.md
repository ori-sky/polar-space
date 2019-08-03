---


---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | remove_first:'/' }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

