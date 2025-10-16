---
layout: post
title: GitHub Commits 提交规范
date: 2025-09-12 10:00:00
description: Conventional Commits 是一种规范化的 git 提交信息格式，遵循规范可以有效提升 commit message 的可读性，也方便历史记录和版本控制。
tags: [Git, Conventional Commits]
categories: engineering
---

## 基本格式

Conventional Commits 的基本格式如下：
```bash
<type>[optional scope]: <description>
[optional body]
[optional footer(s)]
```

其中 `<type>` 表示提交类型（必填），也决定了它在 changelog 中的分类，通常有以下几种：

- `feat`: 新功能；
- `fix`: 修复 bug；
- `docs`: 仅修改文档；
- `style`: 不影响代码逻辑的修改，比如格式、空格、缩进、缺失的分号；
- `refactor`: 代码重构（不包含功能变更或 bug 修复）；
- `perf`: 性能优化；
- `test`: 添加测试或修改测试；
- `build`: 构建系统或依赖的变动（例如 webpack、rollup）
- `chore`: 杂项、不属于其他类型的更改（比如改 .gitignore、更新依赖）；
- `ci`: 持续集成相关（GitHub Actions、Travis CI、Circle 等）；
- `revert`: 回滚某个提交（会自动生成 footer）。

更多内容请见 [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) 。
