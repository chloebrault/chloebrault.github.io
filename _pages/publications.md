---
layout: page
permalink: /publications/
title: talks and publications
description: sample of talks and public writing
nav: true
nav_order: 3
published: true
---

<!-- _pages/publications.md -->

<div class="publications">

<h1>publications</h1>

{% bibliography -f publication %}

<h1>talks</h1>

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f talk -q @*[year={{y}}]* %}
{% endfor %}

</div>