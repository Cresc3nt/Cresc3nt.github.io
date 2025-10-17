---
layout: post
title: 带有自定义引用块的文章
date: 2023-05-12 15:53:00-0400
description: 带有自定义引用块的博客文章示例
tags: [格式化, 引用块]
categories: [示例文章]
giscus_comments: true  # 启用Giscus评论系统
related_posts: true    # 显示相关文章
---

这篇文章展示了如何为引用块添加自定义样式。基于[jekyll-gitbook](https://github.com/sighingnow/jekyll-gitbook)实现。

我们决定支持与[jekyll-gitbook](https://sighingnow.github.io/jekyll-gitbook/jekyll/2022-06-30-tips_warnings_dangers.html)中相同的自定义引用块，这些样式也在许多其他网站样式中找到。样式定义可以在[\_base.scss](https://github.com/alshedivat/al-folio/blob/main/_sass/_base.scss)文件中找到，更具体地说：

```scss
/* 提示、警告和危险块 */
.post .post-content blockquote {
  &.block-tip {
    border-color: var(--global-tip-block);
    background-color: var(--global-tip-block-bg);

    p {
      color: var(--global-tip-block-text);
    }

    h1,
    h2,
    h3,
    h4,
    h5,
    h6 {
      color: var(--global-tip-block-title);
    }
  }

  &.block-warning {
    border-color: var(--global-warning-block);
    background-color: var(--global-warning-block-bg);

    p {
      color: var(--global-warning-block-text);
    }

    h1,
    h2,
    h3,
    h4,
    h5,
    h6 {
      color: var(--global-warning-block-title);
    }
  }

  &.block-danger {
    border-color: var(--global-danger-block);
    background-color: var(--global-danger-block-bg);

    p {
      color: var(--global-danger-block-text);
    }

    h1,
    h2,
    h3,
    h4,
    h5,
    h6 {
      color: var(--global-danger-block-title);
    }
  }
}
```

普通的引用块可以如下使用：

```markdown
> 这是一个普通的引用块，
> 可以像往常一样使用。
```

> 这是一个普通的引用块，
> 可以像往常一样使用。

这些自定义样式可以通过为引用块添加特定的 class 来使用，如下所示：

<!-- prettier-ignore-start -->

```markdown
> ##### 提示
>
> 当你想就某项内容提供建议时，
> 可以使用提示块。
{: .block-tip }
```

> ##### 提示
>
> 当你想就某项内容提供建议时，
> 可以使用提示块。
{: .block-tip }

```markdown
> ##### 警告
>
> 这是一个警告，应在你想提醒用户
> 注意某些事项时使用。
{: .block-warning }
```

> ##### 警告
>
> 这是一个警告，应在你想提醒用户
> 注意某些事项时使用。
{: .block-warning }

```markdown
> ##### 危险
>
> 这是一个危险区域，应谨慎使用。
{: .block-danger }
```

> ##### 危险
>
> 这是一个危险区域，应谨慎使用。
{: .block-danger }

<!-- prettier-ignore-end -->
