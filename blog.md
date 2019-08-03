---
title: Stuff What We Wrote
layout: blog
---

{% for post in site.posts %}
	<div class="content-box">
		<h1>{{ post.title }}</h1>
		<section>
		</section>
	</div>
{% endfor %}

<!--
<ul>
	{% for post in site.posts %}
		<li>
			<a href="{{ post.url | remove_first:'/' }}">{{ post.title }}</a>
			{{ post.decription }} </li>
	{% endfor %}
</ul>
-->

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcxODE2NzQsLTQ2NzA3ODkxNV19
-->