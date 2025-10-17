---
layout: post
title: 带有高级图片组件的文章
date: 2024-01-27 11:46:00
description: 这是高级图片组件的样子
tags: [格式化, 图片]
categories: [示例文章]
thumbnail: assets/img/9.jpg  # 文章缩略图
images:  # 图片功能配置
  compare: true  # 启用图片对比功能
  slider: true   # 启用图片滑块功能
---

这是一篇带有高级图片组件的示例文章。

## 图片滑块

这是一个简单的图片滑块。它使用[Swiper](https://swiperjs.com/)库。查看[示例页面](https://swiperjs.com/demos)了解更多关于您可以用它实现的功能。

<swiper-container keyboard="true" navigation="true" pagination="true" pagination-clickable="true" pagination-dynamic-bullets="true" rewind="true">
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/blogs/2024-01-27-advanced-images/9.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/blogs/2024-01-27-advanced-images/7.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/blogs/2024-01-27-advanced-images/8.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/blogs/2024-01-27-advanced-images/10.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/blogs/2024-01-27-advanced-images/12.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
</swiper-container>

## 图片对比滑块

这是一个简单的图片对比滑块。它使用[img-comparison-slider](https://img-comparison-slider.sneas.io/)库。查看[示例页面](https://img-comparison-slider.sneas.io/examples.html)了解更多关于您可以用它实现的功能。

<img-comparison-slider>
  {% include figure.liquid path="assets/img/blogs/2024-01-27-advanced-images/prof_pic.jpg" class="img-fluid rounded z-depth-1" slot="first" %}
  {% include figure.liquid path="assets/img/blogs/2024-01-27-advanced-images/prof_pic_color.png" class="img-fluid rounded z-depth-1" slot="second" %}
</img-comparison-slider>
