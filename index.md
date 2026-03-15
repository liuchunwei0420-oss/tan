---
layout: default
title: "唐詩三百首目錄"
---

<style>
  /* 頁籤按鈕樣式 */
  .tab-container { margin: 20px 0; border-bottom: 2px solid #e0dcd3; }
  .tab-btn {
    padding: 10px 20px;
    cursor: pointer;
    border: none;
    background: #fdfaf2;
    font-size: 1.1em;
    font-family: serif;
    color: #666;
  }
  .tab-btn.active {
    background: #c0392b; /* 印章紅 */
    color: white;
    font-weight: bold;
    border-radius: 5px 5px 0 0;
  }
  
  /* 分類區塊樣式 */
  .category-section { display: none; padding: 20px 0; }
  .category-section.active { display: block; }
  
  .group-title {
    color: #c0392b;
    border-bottom: 1px solid #eee;
    padding-bottom: 5px;
    margin-top: 30px;
  }
  .poem-list { list-style: none; padding-left: 10px; }
  .poem-list li { margin: 8px 0; }
  .poem-list a { text-decoration: none; color: #2c3e50; }
  .poem-list a:hover { color: #c0392b; text-decoration: underline; }
  .count { font-size: 0.8em; color: #999; margin-left: 10px; }
</style>

<h1>唐詩三百首目錄</h1>

<div class="tab-container">
  <button class="tab-btn active" onclick="showTab('author-tab', this)">按作者分類</button>
  <button class="tab-btn" onclick="showTab('type-tab', this)">按詩體分類</button>
</div>

<div id="author-tab" class="category-section active">
  {% assign poems_by_author = site.poems | group_by: "author" %}
  {% for group in poems_by_author %}
    <h3 class="group-title">{{ group.name }} <span class="count">({{ group.items.size }}首)</span></h3>
    <ul class="poem-list">
      {% for item in group.items %}
        <li><a href="{{ item.url | relative_url }}">{{ item.title }}</a></li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>

<div id="type-tab" class="category-section">
  {% assign poems_by_type = site.poems | group_by: "type" %}
  {% for group in poems_by_type %}
    <h3 class="group-title">{{ group.name }} <span class="count">({{ group.items.size }}首)</span></h3>
    <ul class="poem-list">
      {% for item in group.items %}
        <li><a href="{{ item.url | relative_url }}">{{ item.title }}</a> <span style="color:#999">- {{ item.author }}</span></li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>

<script>
  function showTab(tabId, btn) {
    // 隱藏所有區塊
    document.querySelectorAll('.category-section').forEach(sec => sec.classList.remove('active'));
    // 取消所有按鈕活性
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    
    // 顯示目標區塊
    document.getElementById(tabId).classList.add('active');
    // 啟動當前按鈕
    btn.classList.add('active');
  }
</script>
