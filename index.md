<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'functional-principles-khorikov' %}
      <li>
        <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
      </li>
     {% endif %}
  {% endfor %}
</ul>

