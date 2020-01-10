---
title: Lorenzo Negri
---

## Lorenzo Negri: R Computational Thinking

This is a collection of R laboratories and case studies of my continuous improving and learning in Data Science field with R; visualizations, statistics, analysis and other stuff.

<div class="toc" markdown="1">
## Case Study:

{% for case in site.pages %}
{% if case.cas == true %}- [{{ topic.title }}]({{ topic.url | absolute_url }}){% endif %}
{% endfor %}
</div>

<div class="toc" markdown="1">
## Topics:

{% for topic in site.pages %}
{% if topic.nav == true %}- [{{ topic.title }}]({{ topic.url | absolute_url }}){% endif %}
{% endfor %}
</div>
