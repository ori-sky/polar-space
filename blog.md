---
title: Stuff What We Wrote
layout: blog
---

{::nomarkdown}
{% for post in site.posts %}
	<div class="small-4 columns">
		<a href="{{ post.url | remove_first:'/' }}">
			<div class="content-box blog-preview">
				<h>{{ post.title }}</h3>
				<h4>by {{ post.author }}</h4
			>
			</div>
		</a>
	</div>
{% endfor %}
{:/}

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIwNTQyMjU0LC0xMTczOTU4NzUwLC03MT
gxNjc0LC00NjcwNzg5MTVdfQ==
-->