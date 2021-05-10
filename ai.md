---
layout: page
titles:
  # @start locale config
  en      : &EN       AI
  en-GB   : *EN
  en-US   : *EN
  en-CA   : *EN
  en-AU   : *EN
  ko      : &KO       인공지능
  ko-KR   : *KO
  # @end locale config
key: page-ai
---

<div id="archives">
{% for post in site.categories.AI %}
<li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>
