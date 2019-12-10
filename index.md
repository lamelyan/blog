<ul>
  {% for post in site.posts | where: "tags","functional-principles-khorikov" %}
    <li>
      <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

