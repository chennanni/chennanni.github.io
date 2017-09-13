---
layout: page
title: Blog
permalink: /blog/
order: 1
---

<h3>最近更新</h3>

<ul class="post-list">
  <li>
    <span class="post-meta">2017-now</span>
    <h2><a class="post-link" href="http://www.jianshu.com/nb/7671820">简书</a></h2>
  </li>
  <li>
    <span class="post-meta">2017-now</span>
    <h2><a class="post-link" href="http://www.cnblogs.com/maxstack">博客园</a></h2>
  </li>
</ul>

<h3>技术</h3>

<ul class="post-list">
  {% for post in site.categories.tech-cn %}
    <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

      <h2>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>
    </li>
  {% endfor %}
</ul>

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

<h3>Article</h3>

<ul class="post-list">
  {% for post in site.categories.blog %}
    <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

      <h2>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>
    </li>
  {% endfor %}
</ul>

<h3>往期文章</h3>

<ul class="post-list">
  <li>
    <span class="post-meta">2012</span>
    <h2><a class="post-link" href="http://blog.sina.com.cn/s/articlelist_2767001672_0_1.html">新浪博客</a></h2>
  </li>
  <li>
    <span class="post-meta">2009-2012</span>
    <h2><a class="post-link" href="http://396185801.qzone.qq.com">QQ空间</a></h2>
  </li>
</ul>

<!-- <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p> -->
