---
layout: base
---
<ul>
  {% for page in site.pages %}
    {% if page.brand %}
      <li>
        <a href="{{ site.base_url }}{{ page.url | split:".html" }}">
          {{ page.brand }}
        </a>
      </li>
    {% endif %}
  {% endfor %}
</ul>
