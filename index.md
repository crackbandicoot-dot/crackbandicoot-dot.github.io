---
layout: home
title: Welcome to My Curriculum
---

# Hi, I'm Cristhian Delgado

This site showcases my projects, skills, and learning journey.  
All content is written in Markdown and automatically turned into HTML by Jekyll.

## Projects

{% for project in site.projects %}
- **[{{ project.title }}]({{ project.url | relative_url }})**  
  {{ project.description }}
{% endfor %}

---

*Want to see more? Check out my [GitHub](https://github.com/crackbandicoot-dot).*
