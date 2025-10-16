---
layout: post
title: 带有图片画廊的文章
date: 2024-12-04 01:59:00
description: 这是包含图片画廊的样子
tags: [格式化, 图片]
categories: [示例文章]
thumbnail: assets/img/9.jpg  # 文章缩略图
images:  # 图片画廊功能配置
  lightbox2: true    # 启用Lightbox2
  photoswipe: true   # 启用PhotoSwipe
  spotlight: true    # 启用Spotlight
  venobox: true      # 启用Venobox
---

这篇文章中的图片都是可缩放的，使用不同的库排列成不同的迷你画廊。

## [Lightbox2](https://lokeshdhakar.com/projects/lightbox2/)

<a href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/1/img-2500.jpg" data-lightbox="roadtrip"><img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/1/img-200.jpg" /></a>
<a href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-2500.jpg" data-lightbox="roadtrip"><img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-200.jpg" /></a>
<a href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-2500.jpg" data-lightbox="roadtrip"><img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-200.jpg" /></a>

---

## [PhotoSwipe](https://photoswipe.com/)

<div class="pswp-gallery pswp-gallery--single-column" id="gallery--getting-started">
  <a href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-2500.jpg"
    data-pswp-width="1669"
    data-pswp-height="2500"
    target="_blank">
    <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-200.jpg" alt="" />
  </a>
  <!-- cropped thumbnail: -->
  <a href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/7/img-2500.jpg"
    data-pswp-width="1875"
    data-pswp-height="2500"
    data-cropped="true"
    target="_blank">
    <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/7/img-200.jpg" alt="" />
  </a>
  <!-- data-pswp-src with custom URL in href -->
  <a href="https://unsplash.com"
    data-pswp-src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-2500.jpg"
    data-pswp-width="2500"
    data-pswp-height="1666"
    target="_blank">
    <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-200.jpg" alt="" />
  </a>
  <!-- wrapped with any element: -->
  <div>
    <a href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/6/img-2500.jpg"
      data-pswp-width="2500"
      data-pswp-height="1667"
      target="_blank">
      <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/6/img-200.jpg" alt="" />
    </a>
  </div>
</div>

---

## [Spotlight JS](https://nextapps-de.github.io/spotlight/)

<!-- Group 1 -->
<div class="spotlight-group">
    <a class="spotlight" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/1/img-2500.jpg">
        <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/1/img-200.jpg" />
    </a>
    <a class="spotlight" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-2500.jpg">
        <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-200.jpg" />
    </a>
    <a class="spotlight" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-2500.jpg">
        <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-200.jpg" />
    </a>
</div>
<!-- Group 2 -->
<div class="spotlight-group">
    <a class="spotlight" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/4/img-2500.jpg">
        <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/4/img-200.jpg" />
    </a>
    <a class="spotlight" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/5/img-2500.jpg">
        <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/5/img-200.jpg" />
    </a>
    <a class="spotlight" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/6/img-2500.jpg">
        <img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/6/img-200.jpg" />
    </a>
</div>

---

## [Venobox](https://veno.es/venobox/)

<a class="venobox" data-gall="myGallery" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/1/img-2500.jpg"><img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/1/img-200.jpg" /></a>
<a class="venobox" data-gall="myGallery" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-2500.jpg"><img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-200.jpg" /></a>
<a class="venobox" data-gall="myGallery" href="https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-2500.jpg"><img src="https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-200.jpg" /></a>
