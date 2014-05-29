---
layout: index
---

<div class="home">

  {% for cat in site.category-list %}
    <h2>{{ cat }}</h2>
    <ul>
    {% for page in site.pages %}
      {% if page.resource == true %}
        {% for pc in page.categories %}
          {% if pc == cat %}
          <li><a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a></li>
          {% endif %}   <!-- cat-match-p -->
        {% endfor %}  <!-- page-category -->
      {% endif %}   <!-- resource-p -->
    {% endfor %} <!-- page -->

    </ul>
  {% endfor %}  <!-- cat -->

</div>
