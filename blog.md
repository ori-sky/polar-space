---
title: Stuff What We Wrote
layout: blog
---

{::nomarkdown}
{% for post in site.posts %}
	<a href="{{ post.url | remove_first:'/' }}">
		<div class="content-box">
			<h1>{{ post.title }}</h1>
			<section>
				{{ post.description }}
			</section>
		</div>
	</a>
{% endfor %}
{:/}

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcxODE2NzQsLTQ2NzA3ODkxNV19
-->
