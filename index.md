---
title: hem
layout: withnav
order: 1
---

{% comment %}
![stout in the woods](/images/pexels-eva-elijas-5659716-cr.jpg)
{% endcomment %}




<div>
    <p>
      Vi är en svensk importör av kvalitetsöl. Amerikanska hantverksipor, traditionella tyska stilar, experimentell suröl... Vi gillar allt som är gediget, genomtänkt <b>och gott!</b></p>
  </div>



 {% include news_list.html collection=site.data.home.news %}

{% if site.theme_config.show_projects == true %}
  <h2>{{ site.theme_config.home.title_projects }}</h2>
  {% include card_list.html collection=site.data.home.project_entries %}
{% endif %}

{% if site.theme_config.show_misc_list == true %}
  <h2>{{ site.theme_config.home.title_misc_list }}</h2>
  {% include vertical_list.html collection=site.data.home.misc_entries %}
{% endif %}

{% if site.theme_config.show_blog == true %}
  <h2>{{ site.theme_config.home.title_blog }}</h2>
  {% include post_list.html %}
{% endif %}

{% if site.theme_config.show_old_projects == true %}
  <h2>{{ site.theme_config.home.title_old_projects }}</h2>
  {% include card_list.html collection=site.data.home.old_project_entries %}
{% endif %}


