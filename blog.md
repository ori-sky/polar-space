---
title: Stuff What We Wrote
layout: blog
---

{::nomarkdown}
{% for post in site.posts %}
	<div class="small-4 columns">
		<a href="{{ post.url | remove_first:'/' }}">
			<div class="content-box blog-preview">
				<h3>{{ post.title }}</h3>
				by {{ post.author }}
				<section>
					{{ post.description }}
				</section>
			</div>
		</a>
	</div>
{% endfor %}
{:/}

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzM5NTg3NTAsLTcxODE2NzQsLTQ2Nz
A3ODkxNV19
-->
