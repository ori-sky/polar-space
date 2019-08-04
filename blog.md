---
title: Stuff What We Wrote
layout: blog
---

{::nomarkdown}
{% for post in site.posts %}
	<div class="medium-4 small-12 columns">
		<a href="{{ post.url | remove_first:'/' }}">
			<div class="content-box blog-preview">
				<h3>{{ post.title }}</h3>
				<h4>by {{ post.author }} on {{ post.date | date: "%Y-%m-%d" }}</h4>
				<section>
					{{ post.description }}
				</section>
			</div>
		</a>
	</div>
{% endfor %}
{:/}

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIwNTQyMjU0LC0xMTczOTU4NzUwLC03MT
gxNjc0LC00NjcwNzg5MTVdfQ==
-->
