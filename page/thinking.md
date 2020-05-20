---
layout: page
title: 杂谈
permalink: /thinking/
order: 2
---

<h3>杂谈</h3>

<ul class="post-list">
  {% for post in site.categories.mumble-cn %}
    <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

      <h2>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>
    </li>
  {% endfor %}
</ul>

<!-- <h3>老文章</h3>

<ul class="post-list">
  <li>
    <span class="post-meta">2012</span>
    <h2><a class="post-link" href="http://blog.sina.com.cn/s/articlelist_2767001672_0_1.html">新浪博客（隐藏）</a></h2>
  </li>
  <li>
    <span class="post-meta">2009-2012</span>
    <h2><a class="post-link" href="http://396185801.qzone.qq.com">QQ空间</a></h2>
  </li>
</ul> -->
