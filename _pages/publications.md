---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---


You can also find my publications in <a href="https://www.researchgate.net/profile/Thi_Hoa_Nguyen4">ResearchGate</a>. <!-- and on <a href="https://scholar.google.com/">my Google Scholar profile</a>. -->


{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
