---
layout: home
author_profile: true
title: Welcome to My Curriculum
---

# Hi, I'm [Cristhian Delgado]

This site showcases my projects, skills, and learning journey.  
All content is written in Markdown and automatically turned into HTML by Jekyll.

## Projects

ul>
{% for project in site.projects %}
  <li>
    <a href="{{ project.url | relative_url }}">{{ project.title }}</a> – {{ project.description }}
  </li>
{% endfor %}
</ul>
---

*Want to see more? Check out my [GitHub](https://github.com/crackbandicoot-dot).*
