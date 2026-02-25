---
layout: page
title: People
---

## Current Lab Members

{% assign sorted = site.people | sort: 'order' | reverse %}
{% for person in sorted %}
{% if person.category == "current" %}
  {% assign person_key = person.first_name | append: " " | append: person.last_name %}
  <div class="person">
  	<img src="{{ person.profile_pic }}" alt="Image of {{ person.name }}" height="175px">
  	<br>
  	<span class="name">{{ person.name }}</span>
  	<span class="role">{{ person.role }}</span>
  	<a href="{{ person.website }}"><i class="fas fa-home"></i></a>
  	<a href="mailto:{{ person.email }}"><i class="fas fa-envelope"></i></a>
  	<a href="{{ "/publications.html" | relative_url }}?author={{ person_key | url_encode }}" title="View publications"><i class="fas fa-book"></i></a>
	<p>{{ staff_member.content | markdownify }}</p>
  </div>
{% endif %}
{% endfor %}

<div style="clear:both"></div>

## Past Lab Members

{% for person in sorted %}
{% if person.category == "past" %}
  {% assign person_key = person.first_name | append: " " | append: person.last_name %}
  <div class="person">
  	<img src="{{ person.profile_pic }}" alt="Image of {{ person.name }}" height="175px">
  	<br>
  	<span class="name">{{ person.name }}</span>
  	<span class="role">{{ person.role }}</span>
  	<a href="{{ person.website }}"><i class="fas fa-home"></i></a>
  	<a href="mailto:{{ person.email }}"><i class="fas fa-envelope"></i></a>
  	<a href="{{ "/publications.html" | relative_url }}?author={{ person_key | url_encode }}" title="View publications"><i class="fas fa-book"></i></a>
	<p>{{ staff_member.content | markdownify }}</p>
  </div>
{% endif %}
{% endfor %}

<div style="clear:both"></div>
