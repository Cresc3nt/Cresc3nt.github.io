---
layout: post
title: 带有伪代码的文章
date: 2024-04-15 00:01:00
description: 这是包含伪代码的样子
tags: [格式化, 代码]
categories: [示例文章]
pseudocode: true  # 启用伪代码渲染
---

这是一篇包含一些由[pseudocode](https://github.com/SaswatPadhi/pseudocode.js)渲染的伪代码的示例文章。这里展示的示例与[pseudocode.js](https://saswat.padhi.me/pseudocode.js/)文档中的示例相同，只有一个简单但重要的变化：每当您使用`$`时，您应该使用`$$`代替。另外，请注意前置元数据中的`pseudocode`键设置为`true`以启用伪代码的渲染。例如，使用此代码：

````markdown
```pseudocode
% 以下快速排序算法摘自《算法导论》（第3版）第7章
\begin{algorithm}
\caption{快速排序}
\begin{algorithmic}
\PROCEDURE{Quicksort}{$$A, p, r$$}
    \IF{$$p < r$$}
        \STATE $$q = $$ \CALL{Partition}{$$A, p, r$$}
        \STATE \CALL{Quicksort}{$$A, p, q - 1$$}
        \STATE \CALL{Quicksort}{$$A, q + 1, r$$}
    \ENDIF
\ENDPROCEDURE
\PROCEDURE{Partition}{$$A, p, r$$}
    \STATE $$x = A[r]$$
    \STATE $$i = p - 1$$
    \FOR{$$j = p$$ \TO $$r - 1$$}
        \IF{$$A[j] < x$$}
            \STATE $$i = i + 1$$
            \STATE 交换 $$A[i]$$ 与 $$A[j]$$
        \ENDIF
        \STATE 交换 $$A[i]$$ 与 $$A[r]$$
    \ENDFOR
\ENDPROCEDURE
\end{algorithmic}
\end{algorithm}
```
````

将生成如下结果：

```pseudocode
% 以下快速排序算法摘自《算法导论》（第3版）第7章
\begin{algorithm}
\caption{快速排序}
\begin{algorithmic}
\PROCEDURE{Quicksort}{$$A, p, r$$}
    \IF{$$p < r$$}
        \STATE $$q = $$ \CALL{Partition}{$$A, p, r$$}
        \STATE \CALL{Quicksort}{$$A, p, q - 1$$}
        \STATE \CALL{Quicksort}{$$A, q + 1, r$$}
    \ENDIF
\ENDPROCEDURE
\PROCEDURE{Partition}{$$A, p, r$$}
    \STATE $$x = A[r]$$
    \STATE $$i = p - 1$$
    \FOR{$$j = p$$ \TO $$r - 1$$}
        \IF{$$A[j] < x$$}
            \STATE $$i = i + 1$$
            \STATE 交换 $$A[i]$$ 与 $$A[j]$$
        \ENDIF
        \STATE 交换 $$A[i]$$ 与 $$A[r]$$
    \ENDFOR
\ENDPROCEDURE
\end{algorithmic}
\end{algorithm}
```
