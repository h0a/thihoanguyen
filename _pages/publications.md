---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---


You can also find my publications in <a href="https://www.researchgate.net/profile/Thi_Hoa_Nguyen4">ResearchGate</a> and on my <a href="https://scholar.google.de/citations?hl=de&user=B0J3wBwAAAAJ">Google Scholar profile</a>.


{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
