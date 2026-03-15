---
layout: default
title: "唐詩三百首目錄"
---

<style>
  /* 基礎配色 */
  :root {
    --ink-black: #2c3e50;
    --seal-red: #c0392b;
    --paper-bg: #fdfaf2;
    --gray-text: #999;
  }

  /* 搜尋框樣式 */
  .search-wrapper {
    margin: 20px 0;
  }
  #searchInput {
    width: 100%;
    padding: 12px 15px;
    font-size: 1.1em;
    border: 2px solid #e0dcd3;
    border-radius: 5px;
    background-color: #fff;
    font-family: serif;
    box-sizing: border-box;
  }
  #searchInput:focus {
    border-color: var(--seal-red);
    outline: none;
  }

  /* 頁籤按鈕樣式 */
  .tab-container { 
    margin: 25px 0 10px 0; 
    border-bottom: 2px solid #e0dcd3; 
  }
  .tab-btn {
    padding: 10px 25px;
    cursor: pointer;
    border: none;
    background: var(--paper-bg);
    font-size: 1.1em;
    font-family: serif;
    color: #666;
    transition: 0.3s;
  }
  .tab-btn.active {
    background: var(--seal-red);
    color: white;
    font-weight: bold;
    border-radius: 5px 5px 0 0;
  }
  
  /* 分類區塊樣式 */
  .category-section { display: none; }
  .category-section.active { display: block; }
  
  .group-title {
    color: var(--seal-red);
    border-bottom: 1px solid #eee;
    padding-bottom: 5px;
    margin-top: 30px;
  }
  .poem-list { list-style: none; padding-left: 5px; }
  .poem-list li { 
    margin: 10px 0; 
    padding: 5px;
    border-bottom: 1px dashed #eee;
  }
  .poem-list a { 
    text-decoration: none; 
    color: var(--ink-black); 
    font-size: 1.1em;
    font-weight: 500;
  }
  .poem-list a:hover { color: var(--seal-red); }
  .meta-info { font-size: 0.9em; color: var(--gray-text); margin-left: 10px; }
  .count { font-size: 0.8em; color: var(--gray-text); margin-left: 10px; font-weight: normal; }

  /* 搜尋無結果提示 */
  #noResults {
    display: none;
    text-align: center;
    padding: 40px;
    color: var(--gray-text);
    font-size: 1.2em;
  }
</style>

<h1>唐詩三百首目錄</h1>

<div class="search-wrapper">
  <input type="text" id="searchInput" placeholder="輸入詩名、作者、詩體或內容關鍵字..." onkeyup="searchPoems()">
</div>

<div class="tab-container">
  <button class="tab-btn active" onclick="showTab('author-tab', this)">按作者分類</button>
  <button class="tab-btn" onclick="showTab('type-tab', this)">按詩體分類</button>
</div>

<div id="noResults">找不到相關詩詞，請換個關鍵字試試。</div>

<div id="author-tab" class="category-section active">
  {% assign poems_by_author = site.poems | group_by: "author" %}
  {% for group in poems_by_author %}
    <div class="group-wrapper">
      <h3 class="group-title">{{ group.name }} <span class="count">({{ group.items.size }}首)</span></h3>
      <ul class="poem-list">
        {% for item in group.items %}
          <li class="poem-item">
            <a href="{{ item.url | relative_url }}">{{ item.title }}</a>
            <span class="meta-info">- {{ item.type }}</span>
          </li>
        {% endfor %}
      </ul>
    </div>
  {% endfor %}
</div>

<div id="type-tab" class="category-section">
  {% assign poems_by_type = site.poems | group_by: "type" %}
  {% for group in poems_by_type %}
    <div class="group-wrapper">
      <h3 class="group-title">{{ group.name }} <span class="count">({{ group.items.size }}首)</span></h3>
      <ul class="poem-list">
        {% for item in group.items %}
          <li class="poem-item">
            <a href="{{ item.url | relative_url }}">{{ item.title }}</a>
            <span class="meta-info">- {{ item.author }}</span>
          </li>
        {% endfor %}
      </ul>
    </div>
  {% endfor %}
</div>

<script>
  // 切換頁籤功能
  function showTab(tabId, btn) {
    document.querySelectorAll('.category-section').forEach(sec => sec.classList.remove('active'));
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    document.getElementById(tabId).classList.add('active');
    btn.classList.add('active');
    // 切換 Tab 後重新執行搜尋過濾，確保結果正確
    searchPoems();
  }

  // 即時搜尋過濾功能
  function searchPoems() {
    let input = document.getElementById('searchInput').value.toLowerCase();
    let activeTab = document.querySelector('.category-section.active');
    let groups = activeTab.querySelectorAll('.group-wrapper');
    let totalVisible = 0;

    groups.forEach(group => {
      let items = group.querySelectorAll('.poem-item');
      let hasMatch = false;

      items.forEach(li => {
        let text = li.textContent.toLowerCase();
        if (text.includes(input)) {
          li.style.display = "";
          hasMatch = true;
          totalVisible++;
        } else {
          li.style.display = "none";
        }
      });

      // 如果該組別（某作者或某詩體）下沒有符合的詩，隱藏標題
      group.style.display = hasMatch ? "" : "none";
    });

    // 顯示/隱藏「無結果」提示
    document.getElementById('noResults').style.display = (totalVisible === 0) ? "block" : "none";
  }
</script>
