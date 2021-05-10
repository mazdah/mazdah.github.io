---
layout: page
titles:
  # @start locale config
  en      : &EN       Spring
  en-GB   : *EN
  en-US   : *EN
  en-CA   : *EN
  en-AU   : *EN
  ko      : &KO       스프링
  ko-KR   : *KO
  # @end locale config
key: page-spring
---

<div id="archives">
{% for category in site.categories.Spring %}
  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <div id="#{{ category_name | slugize }}"></div>
    <p></p>

    <h3 class="category-head">{{ category_name }}</h3>
    <a name="{{ category_name | slugize }}"></a>
    {% for post in site.categories[category_name] %}
    <article class="archive-item">
      <p style="line-height:1rem">
        <a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a>
        <span style="font-size: 1rem; font-weight:normal">{{post.date | date: "%b %d %Y"}}</span>
      </p>
    </article>
    {% endfor %}
  </div>
{% endfor %}
</div>

