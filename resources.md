---
layout: page
title: Resources - SGX3 Coding Institute 2026
permalink: /resources/
---

# Resources

A categorized reference of all tools, links, and platforms used during the SGX3 Coding Institute & Hackathon 2026.

<div class="resource-controls">
  <div class="search-wrap">
    <svg class="search-icon" xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
    <input type="text" id="resource-search" placeholder="Search by name or description..."
           class="resource-search-input" autocomplete="off" spellcheck="false">
    <button class="search-clear" id="search-clear" aria-label="Clear search" style="display:none">&times;</button>
  </div>
  <div class="jump-wrap">
    <label for="jump-select" class="jump-label">Jump to:</label>
    <select id="jump-select" class="jump-select">
      <option value="">&#8212; Select section &#8212;</option>
      {% for group in site.data.resources %}
      <option value="section-{{ group.category | slugify }}">{{ group.category }}</option>
      {% endfor %}
    </select>
  </div>
</div>

<p id="no-results" class="no-results" style="display:none">No resources match your search.</p>

{% for group in site.data.resources %}
<section class="resource-section" id="section-{{ group.category | slugify }}">
  <h2>{{ group.category }}</h2>
  <div class="resource-group {{ group.color }}">
    {% for item in group.resources %}
    <div class="resource-item" data-search="{{ item.name | downcase }} {{ item.description | downcase }}{% if item.code %} {{ item.code | downcase }}{% endif %}">
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
</section>
{% endfor %}

<p class="footer-note">Resources compiled from Coding Institute materials and chat logs. Links open in a new tab.</p>

<script>
(function () {
  var searchInput = document.getElementById('resource-search');
  var clearBtn    = document.getElementById('search-clear');
  var jumpSelect  = document.getElementById('jump-select');
  var noResults   = document.getElementById('no-results');

  function filter(query) {
    var q     = query.toLowerCase().trim();
    var total = 0;

    document.querySelectorAll('.resource-section').forEach(function (section) {
      var items   = section.querySelectorAll('.resource-item');
      var visible = 0;

      items.forEach(function (item) {
        var match = !q || (item.dataset.search || '').indexOf(q) !== -1;
        item.style.display = match ? '' : 'none';
        if (match) visible++;
      });

      section.style.display = (visible > 0 || !q) ? '' : 'none';
      total += visible;
    });

    noResults.style.display   = (q && total === 0) ? 'block' : 'none';
    clearBtn.style.display    = q ? 'flex' : 'none';
  }

  searchInput.addEventListener('input', function () {
    filter(this.value);
  });

  clearBtn.addEventListener('click', function () {
    searchInput.value = '';
    filter('');
    searchInput.focus();
  });

  jumpSelect.addEventListener('change', function () {
    if (!this.value) return;
    var target = document.getElementById(this.value);
    if (target) target.scrollIntoView({ behavior: 'smooth', block: 'start' });
    var sel = this;
    setTimeout(function () { sel.value = ''; }, 600);
  });
})();
</script>
