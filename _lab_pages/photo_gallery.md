---
layout: page
title: Lab Photo Gallery
date:   2024-8-15
author: Christopher J. MacLellan
comments: false
---

We like to do fun things and sometimes we take pictures of them...

<div class="gallery">
  {%- for image in site.static_files -%}
    {%- if image.path contains '/photos/' -%}
      <div class="photo">
        <a href="{{ image.path | relative_url }}" target="_blank">
          <img src="{{ image.path | relative_url }}" alt="{{ image.name }}">
        </a>
      </div>
    {%- endif -%}
  {%- endfor -%}
</div>

<style>
  .gallery {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }
  .photo img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  }
  .photo {
    flex: 1 1 calc(33.333% - 10px);
  }
</style>

