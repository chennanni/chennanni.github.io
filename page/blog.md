---
layout: page
title: Blog
permalink: /blog/
order: 3
---

<h3>最近更新</h3>

<ul class="post-list">
  <li>
    <h2><a class="post-link" href="http://www.jianshu.com">新浪博客</a></h2>
  </li>
  <li>
    <h2><a class="post-link" href="https://www.cnblogs.com">QQ空间</a></h2>
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
    <h2><a class="post-link" href="www.jianshu.com">新浪博客</a></h2>
  </li>
  <li>
    <h2><a class="post-link" href="www.jianshu.com">QQ空间</a></h2>
  </li>
</ul>

<!-- <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p> -->
