---
layout: default
title: "唐詩三百首首頁"
---

# 歡迎來到唐詩三百首

這裡收錄了經典的唐詩作品，每首詩都附有白話新譯與平仄分析。

### 詩詞列表
<ul>
  {% for poem in site.poems %}
    <li>
      <a href="{{ poem.url | relative_url }}">【{{ poem.author }}】{{ poem.title }}</a>
    </li>
  {% endfor %}
</ul>

{% if site.poems.size == 0 %}
<p style="color: red;">⚠️ 目前系統還沒抓到 _poems 資料夾裡的檔案，請稍候或檢查檔案格式。</p>
{% endif %}
