---
layout: post
title: 带有Jupyter Notebook的文章
date: 2023-07-04 08:57:00-0400
description: 带有Jupyter Notebook的博客文章示例
tags: [格式化, jupyter]
categories: [示例文章]
giscus_comments: true  # 启用Giscus评论系统
related_posts: false   # 不显示相关文章
---

要在文章中包含Jupyter Notebook，您可以使用以下代码：

{% raw %}

```liquid
{::nomarkdown}
{% assign jupyter_path = 'assets/jupyter/blogs/2023-07-04-jupyter-notebook/blog.ipynb' | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/blogs/2023-07-04-jupyter-notebook/blog.ipynb %}{% endcapture %}
{% if notebook_exists == 'true' %}
  {% jupyter_notebook jupyter_path %}
{% else %}
  <p>抱歉，您要查找的 Notebook 不存在。</p>
{% endif %}
{:/nomarkdown}
```

{% endraw %}

我们来分解一下：这一功能得益于 [Jekyll Jupyter Notebook 插件](https://github.com/red-data-tools/jekyll-jupyter-notebook) ，它允许您将 Jupyter Notebook 嵌入到文章中。该插件本质上会调用 [`jupyter nbconvert --to html`](https://nbconvert.readthedocs.io/en/latest/usage.html#convert-html) ，将 Notebook 转换为 HTML 页面，然后将其嵌入到文章中。由于 [Kramdown](https://jekyllrb.com/docs/configuration/markdown/) 是 Jekyll 默认的 Markdown 渲染器，我们需要用 [::nomarkdown](https://kramdown.gettalong.org/syntax.html#extensions) 标签将插件调用包裹起来，以防止 Kramdown 对该部分内容进行处理，从而原样输出内容。

该插件接收 Notebook 的路径作为输入，但它默认文件一定存在。如果您希望在调用插件前先检查文件是否存在，可以使用 `file_exists` 过滤器。这样可以避免因插件找不到文件而返回 404 错误，并防止将网站首页错误地嵌入到该位置。如果文件不存在，您可以向用户显示一条提示信息。上面展示的代码会输出如下内容：

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/blogs/2023-07-04-jupyter-notebook/blog.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/blogs/2023-07-04-jupyter-notebook/blog.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
{% jupyter_notebook jupyter_path %}
{% else %}

<p>抱歉，您要查找的 Notebook 不存在。</p>
{% endif %}
{:/nomarkdown}

请注意，Jupyter Notebook 支持亮色和暗色两种主题。
