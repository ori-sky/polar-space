---
title: Stuff What We Wrote
layout: blog
---

{::nomarkdown}
{% for post in site.posts %}
	<div class="small-4 columns">
		<a href="{{ post.url | remove_first:'/' }}">
			<div class="content-box blog-preview">
				<h3>{{ post.title }}</h
				by {{ post.author }}1>
				<s>
					</section>
			</div>
		</a>
	</div>
{% endfor %}
{:/}

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI0MjA4OTc5MSwtMTE3Mzk1ODc1MCwtNz
E4MTY3NCwtNDY3MDc4OTE1XX0=
-->