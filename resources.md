---
layout: page
title: Resources - SGX3 Coding Institute 2026
permalink: /resources/
---

# Resources

A categorized reference of all tools, links, and platforms used during the SGX3 Coding Institute & Hackathon 2026.

---

{% for group in site.data.resources %}
## {{ group.category }}

<div class="resource-group {{ group.color }}">
  {% for item in group.resources %}
  <div class="resource-item">
    <div class="resource-info">
      <span class="resource-name">{{ item.name }}</span>
      {% if item.description %}<span class="resource-desc">{{ item.description }}</span>{% endif %}
    </div>
    {% if item.url %}
    <a href="{{ item.url }}" class="resource-link" target="_blank" rel="noopener">{{ item.action }} &rarr;</a>
    {% elsif item.code %}
    <code class="resource-code">{{ item.code }}</code>
    {% endif %}
  </div>
  {% endfor %}
</div>

---
{% endfor %}

<p class="footer-note">Resources compiled from Coding Institute materials and chat logs. Links open in a new tab.</p>
