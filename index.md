
   A great Pluralsight course by Vladimir Khorikov on [Applying Functional Principles in C#](https://app.pluralsight.com/library/courses/csharp-applying-functional-principles/).  
   
   I took away a lot of good programming concepts. 

<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'functional-principles-khorikov' %}
      <li>
        <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
      </li>
     {% endif %}
  {% endfor %}
</ul>

