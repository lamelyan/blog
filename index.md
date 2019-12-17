Design Patterns
<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'design-pattern' %}
      <li>
        <a href="{{ post.url | absolute_url}}">{{ post.title }}</a>
      </li>
     {% endif %}
  {% endfor %}
</ul>


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
<script>
  // Check if a new cache is available on page load.
window.addEventListener('load', function(e) {

  window.applicationCache.addEventListener('updateready', function(e) {
    if (window.applicationCache.status == window.applicationCache.UPDATEREADY) {
      // Browser downloaded a new app cache.
      if (confirm('A new version of this site is available. Load it?')) {
        window.location.reload();
      }
    } else {
      // Manifest didn't changed. Nothing new to server.
    }
  }, false);

}, false);
  </script>
