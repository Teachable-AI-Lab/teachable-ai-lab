---
layout: page
title: People
---

## People

{% assign sorted = site.people | sort: 'order' | reverse %}
{% for person in sorted %}
  <div class="person">
  	<img src="{{ person.profile_pic }}" alt="Image of {{ person.name }}" width="175px">
  	<br>
  	<span class="name">{{ person.name }}</span>
  	<span class="role">{{ person.role }}</span>
  	<a href="{{ person.website }}"><i class="fas fa-home"></i></a>
  	<a href="mailto:{{ person.email }}"><i class="fas fa-envelope"></i></a>
	<p>{{ staff_member.content | markdownify }}</p>
  </div>
{% endfor %}

<div style="clear:both"></div>

