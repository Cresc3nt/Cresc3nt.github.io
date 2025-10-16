---
layout: post
title: 带有参考文献的文章
date: 2023-07-12 09:56:00-0400
description: 带有参考文献的博客文章示例
tags: [格式化, 参考文献]
categories: [示例文章]
giscus_comments: true  # 启用Giscus评论系统
related_posts: false   # 不显示相关文章
related_publications: true  # 显示相关出版物
---

这篇文章展示了如何为简单的博客文章添加参考文献。我们支持[jekyll-scholar](https://github.com/inukshuk/jekyll-scholar)支持的所有引用样式。这意味着简单引用如 {% cite einstein1950meaning %}，多重引用如 {% cite einstein1950meaning einstein1905movement %}，长参考文献如 {% reference einstein1905movement %} 或者引用：

{% quote einstein1905electrodynamics %}
Lorem ipsum dolor sit amet, consectetur adipisicing elit,
sed do eiusmod tempor.

Lorem ipsum dolor sit amet, consectetur adipisicing.
{% endquote %}

如果您需要更学术化的内容，请查看[distill风格文章]({% post_url 2018-12-22-distill %})。
