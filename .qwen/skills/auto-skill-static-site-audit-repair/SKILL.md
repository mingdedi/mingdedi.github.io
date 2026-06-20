---
name: static-site-audit-repair
description: 审查并修复从模板 fork 的静态个人主页网站（如 academicpages/Jekyll 主题），包括 SEO 元数据、HTML 语义、导航链接、版权年份等常见问题
source: auto-skill
extracted_at: '2026-06-20T15:31:58.357Z'
---

# 静态个人主页网站审查与修复

适用于审查从模板（如 academicpages.github.io、Minimal Mistakes Jekyll 主题等）fork 的静态个人主页网站。

## 检查清单

### 1. SEO / 元数据（最重要的检查项）

模板 fork 的网站极易遗留以下问题：

- **`<title>`** — 是否为正确站点名称，而非模板名称
- **`<meta property="og:site_name">`** — 是否有模板默认值
- **`<meta property="og:title">`** — 是否有模板描述文本
- **`<link rel="canonical">`** — 是否指向模板站点而非自己的域名
- **`<meta property="og:url">`** — 同上
- **JSON-LD schema** — `name` 是否为 "Your Name"，`url` 是否指向模板
- **RSS feed link** — `href` 和 `title` 是否指向模板站点
- **`<meta itemprop="headline">`** — 是否还是模板描述

### 2. HTML 结构与语义

- **`<html lang="...">`** — 是否与页面实际语言一致（中文站点应为 `zh-CN`）
- **`<h1 class="page__title">`** — 是否为空或仅含空格
- **`<p>` 是否非法嵌套在 `<ul>` 内** — `<ul>` 的直接子元素必须是 `<li>`
- **alt 属性** — 头像/图片的 `alt` 是否为 "Your Sidebar Name" 等占位符
- **空内容区域** — 如 "About" 部分只有标题无正文

### 3. 导航链接

- 子页面（`blogs/`、`commend_web/` 等）中的"关于我"等链接是否指向首页而非对应锚点
- 是否使用绝对 URL 导致环境不兼容

### 4. 版权与时间戳

- 版权年份是否与实际年份相符
- "Site last updated" 日期是否过时

### 5. 功能性问题

- 按钮（如 "Follow"）是否绑定了实际的点击处理
- 主题切换按钮是否有对应的 JavaScript

## 修复步骤

### 1. 读取所有 HTML 文件

```bash
# 读取主页面
read_file: index.html
# 读取子页面
read_file: blogs/index.html
read_file: commend_web/index.html
```

### 2. 按优先级修复

1. **SEO 元数据** — 使用 `edit` 工具逐个替换模板默认值
2. **HTML 结构** — 修复 lang 属性、非法嵌套、空标题、占位符 alt
3. **导航链接** — 将子页面"关于我"等链接改为指向正确的锚点（如 `/#-about-xxx`）
4. **内容填充** — 补充空白的 About 部分（基于已有信息如 sidebar bio）
5. **版权与日期** — 更新年份和最后更新日期
6. **功能按钮** — 移除无功能按钮或绑定处理逻辑

### 3. 提交与推送

```bash
git add -A
git commit -m "<描述性的中文提交信息>"
git push
```

## 注意事项

- 每次修改前先用 `read_file` 确认原始内容，`edit` 工具需要精确匹配 3 行以上上下文
- 不要修改用户未要求的自定义内容（如博客文章的占位标题可以保留）
- 如果站点使用 Git Pages，push 后等待约 1-2 分钟即可在浏览器查看效果
- 版权年份建议同时更新 © 年份和 "Site last updated" 日期
