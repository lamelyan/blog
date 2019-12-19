### Presenters to follow 
A list of people who articulate concepts exceptionally well. 

- Scott Allen <a href="https://odetocode.com/" target="_blank">https://odetocode.com/</a> 
- Mark Seemann <a href="https://blog.ploeh.dk/about/" target="_blank">https://blog.ploeh.dk/about/</a> 
- Vladimir Khorikov <a href="https://enterprisecraftsmanship.com/" target="_blank">https://enterprisecraftsmanship.com/</a> 

### Design Patterns

   A software design pattern is a general, reusable solution to a commonly occurring problem.  
   Here are my notes on some of the design patterns.

<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'design-pattern' %}
      <li>
        <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
      </li>
     {% endif %}
  {% endfor %}
</ul>


### Functional priciples

Great presentation by Vladimir Khorikov on applying functional principles in C#. He demonstrates how this makes your code easier to reason about and increases your confidence that the logic is doing what it suppose to.

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

### Domain-driven design

Yet another great presentation by Vladimir Khorikov on domain-driven design architecture.  This is a great introduction to DDD.  In it he actually demonstrates DDD concepts through code. 

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

### SOLID principles

The best explanation of SOLID principles I've seen so far is
<a href="https://app.pluralsight.com/library/courses/encapsulation-solid/table-of-contents" target="_blank">this pluralsight presentation </a>by Mark Seemann. I will need to jot down a list of takeaways for reference.
