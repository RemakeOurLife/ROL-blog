---
title: 博客搭建完成，从这里开始写第一篇文章
published: 2026-09-23
description: 基于 Astro + Fuwari 的技术博客已经跑起来了。这篇记录日常写作、预览、部署的完整流程。
tags: [Astro, 博客搭建, 教程]
category: 折腾记录
draft: false
---

## 技术选型

这个博客用 **Astro 5** 构建，主题是 [Fuwari](https://github.com/saicaca/fuwari)。选它的原因很直接：

- 写文章只需要 Markdown，不用碰任何前端代码
- 自带标签、分类、归档、全文搜索、暗色模式、目录、RSS
- 构建产物是纯静态文件，托管在 Cloudflare Pages 上零成本

## 新建一篇文章

在 `src/content/posts/` 下新建一个 `.md` 文件即可，文件名会成为网址的一部分。

也可以直接用命令生成：

```bash
pnpm new-post 我的文章标题
```

每篇文章开头的 `---` 之间是元信息（frontmatter）：

```yaml
---
title: 文章标题
published: 2026-09-23
description: 一句话摘要，会显示在文章列表和搜索结果里
tags: [Astro, 前端]
category: 折腾记录
draft: false
---
```

几个要点：

| 字段 | 说明 |
| --- | --- |
| `published` | 发布日期，格式 `YYYY-MM-DD` |
| `tags` | 数组，会被归集成标签页 |
| `category` | 单值，会被归集成分类 |
| `draft` | 设为 `true` 则不会发布，只在本地可见 |

> 写 `draft: true` 是在本地攒稿的最佳方式，构建时会被自动排除。

## 本地预览

```bash
pnpm dev
```

打开 `http://localhost:4321` 就能实时看到效果，改文件会自动刷新。

想验证生产构建（含全文搜索索引）跑：

```bash
pnpm build && pnpm preview
```

注意搜索索引由 Pagefind 在 `build` 阶段生成，所以只有构建后才搜得到内容。

## 支持的写法

主题内置了不少扩展语法，值得用起来：

- **代码块**：支持行号、折叠、高亮标记，语言自动识别
- **数学公式**：用 `$...$` 和 `$$...$$` 书写 LaTeX
- **提示框**：用 `:::note` / `:::tip` / `:::warning` 等指令
- **GitHub 卡片**：粘贴仓库链接会自动渲染成卡片

具体例子可以看文章列表里保留的几篇示例文章，确认无误后删掉即可。

## 部署

代码推到 GitHub 后，在 Cloudflare Pages 里连上仓库，构建设置填：

- 构建命令：`pnpm build`
- 输出目录：`dist`
- Node 版本：`20` 或更高

之后每次 `git push`，Cloudflare 会自动构建并发布，几十秒后新文章就上线了。
