<ul>
  {% for post in site.posts | where: "tag", "functional-principles-khorikov" %}
    <li>
      <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

