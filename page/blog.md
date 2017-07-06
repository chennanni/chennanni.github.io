---
layout: page
title: Blog
permalink: /blog/
order: 3
---

<!-- <h1 class="page-heading">Posts</h1> -->

<h1>最新更新</h1>
<ul>
  <li>
    <h2>简书</h2>
  </li>
  <li>
    <h2>博客园</h2>
  </li>
</ul>

<h1>技术</h1>

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

<h1>杂谈</h1>

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

<h1>Blog</h1>

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

<h1>往期文章</h1>
<ul>
  <li>
    <h2>新浪博客</h2>
  </li>
  <li>
    <h2>QQ空间</h2>
  </li>
</ul>

<!-- <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p> -->
