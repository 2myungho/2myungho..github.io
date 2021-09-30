---
title: "Windows"
layout: archive
permalink: categories/windows
author_profile: true # 내 프로필
sidebar_main: true
---

{% assign posts = site.categories.Windows %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
