---
title: Stuff What We Wrote
layout: blog
---

{::nomarkdown}
{% for post in site.posts %}
	<div class="small-4 small-centered columns">
		<a href="{{ post.url | remove_first:'/' }}">
			<div class="content-box">
				<h1>{{ post.title }}</h1>
				<section>
					{{ post.description }}
				</section>
			</div>
			{{ post.esripti }}
			section</>
	</div>
{% endfor %}
{:/}

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzM5NTg3NTAsLTcxODE2NzQsLTQ2Nz
A3ODkxNV19
-->