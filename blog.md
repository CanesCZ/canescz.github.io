---
layout: default
---

<div class="row education">
	<ul class="col-xs-12 col-sm-11 col-sm-offset-1 col-md-10 col-lg-9 list">
	{% for post in site.posts %}
		<li>
			<article>

				<h1>
					<a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
				</h1>

				<div class="entry">
					{{ post.content | truncatewords: 10}}
				</div>

				<a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
			</article>
		</li>
	{% endfor %}
	</ul>
</div>