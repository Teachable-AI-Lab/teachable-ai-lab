---
layout: page
title: People
---

## Current Lab Members

{% assign sorted = site.people | sort: 'order' | reverse %}
{% for person in sorted %}
{% if person.category == "current" %}
  <div class="person">
  	<img src="{{ person.profile_pic }}" alt="Image of {{ person.name }}" height="175px">
  	<br>
  	<span class="name">{{ person.name }}</span>
  	<span class="role">{{ person.role }}</span>
  	<a href="{{ person.website }}"><i class="fas fa-home"></i></a>
  	<a href="mailto:{{ person.email }}"><i class="fas fa-envelope"></i></a>
	<p>{{ staff_member.content | markdownify }}</p>
  </div>
{% endif %}
{% endfor %}

<div style="clear:both"></div>

## Past Lab Members

{% for person in sorted %}
{% if person.category == "past" %}
  <div class="person">
  	<img src="{{ person.profile_pic }}" alt="Image of {{ person.name }}" height="175px">
  	<br>
  	<span class="name">{{ person.name }}</span>
  	<span class="role">{{ person.role }}</span>
  	<a href="{{ person.website }}"><i class="fas fa-home"></i></a>
  	<a href="mailto:{{ person.email }}"><i class="fas fa-envelope"></i></a>
	<p>{{ staff_member.content | markdownify }}</p>
  </div>
{% endif %}
{% endfor %}

<div style="clear:both"></div>
