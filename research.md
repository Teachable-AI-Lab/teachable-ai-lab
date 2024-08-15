---
layout: page
title: Research
---

## Active Research Projects

<ul class="post-list">
    {% assign sorted = site.projects | sort: 'order' | reverse %}
    {% for project in sorted %}
    {% if project.category == "current" %}

        <li>
            {%- if project.img -%}
            <div style="clear: both;"></div>
            <img class="left list-thumb" src="{{ project.img }}">
            {%- endif -%}

            {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
            <h3>
                <a class="post-link" href="{{ project.url | relative_url }}">
                    {{ project.title | escape }}
                </a>
            </h3>

            {%- if site.show_excerpts -%}
            {{ project.excerpt }}
            {%- endif -%}
            {%- if project.img -%}
            <div style="clear: both;"></div>
            {%- endif -%}
        </li>

    {% endif %}
    {% endfor %}
</ul>

<div style="clear:both"></div>

## Past Research Projects


<ul class="post-list">
    {% assign sorted = site.projects | sort: 'order' | reverse %}
    {% for project in sorted %}
    {% if project.category == "past" %}

        <li>
            {%- if project.img -%}
            <div style="clear: both;"></div>
            <img class="left list-thumb" src="{{ project.img }}">
            {%- endif -%}

            {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
            <h3>
                <a class="post-link" href="{{ project.url | relative_url }}">
                    {{ project.title | escape }}
                </a>
            </h3>

            {%- if site.show_excerpts -%}
            {{ project.excerpt }}
            {%- endif -%}
            {%- if project.img -%}
            <div style="clear: both;"></div>
            {%- endif -%}
        </li>

    {% endif %}
    {% endfor %}
</ul>

<div style="clear:both"></div>
