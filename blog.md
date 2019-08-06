---
title: Stuff What We Wrote
layout: blog
---
{% assign posts = site.posts | where_exp: "post", "post.category != 'Draft'" %}
{::nomarkdown}
{% for post in site.posts %}
	<div class="large-4 medium-6 small-12 cellsmall-4 columns">
		<a href="{{ post.url | remove_first:'/' }}">
			<div class="content-box blog-preview">
				<div class="content-title">
					<h3>{{ post.title }}</h3>
					<h4>by {{ post.author }} on {{ post.date | date: "%Y-%m-%d" }}</h4>
				</div>
				<section>
					{{ post.description }}
				</section>
			</div>
		</a>
	</div>
{% endfor %}
{:/}

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzOTA4MDk2OTQsLTk2NTM1Mzg0OSwxMj
A1NDIyNTQsLTExNzM5NTg3NTAsLTcxODE2NzQsLTQ2NzA3ODkx
NV19
-->