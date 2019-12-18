### Design Patterns
   A software design pattern is a general, reusable solution to a commonly occurring problem.  Here are my notes on some of the design patterns

<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'design-pattern' %}
      <li>
        <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
      </li>
     {% endif %}
  {% endfor %}
</ul>


### 


<a href="https://app.pluralsight.com/library/courses/csharp-applying-functional-principles/" target="_blank">Applying Functional Principles in C#</a>
by Vladimir Khorikov
   
Takeaways:

<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'functional-principles-khorikov' %}
      <li>
        <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
      </li>
     {% endif %}
  {% endfor %}
</ul>


<a href="https://app.pluralsight.com/library/courses/domain-driven-design-in-practice/" target="_blank">Domain-Driven Design in Practice</a>
by Vladimir Khorikov

Takeaways:
<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'ddd-khorikov' %}
      <li>
        <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
      </li>
     {% endif %}
  {% endfor %}
</ul>

