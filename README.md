# XinVista

个人技术博客。Hugo + PaperMod，部署在 GitHub Pages。

## 目录结构

```
content/posts/           文章（Markdown，日常只改这里）
content/about.md         关于页
assets/css/extended/     自定义样式（配色、字体、细节）
static/fonts/            自托管字体：Roboto + JetBrains Mono
static/images/           文章配图
config/_default/         站点配置：站名、菜单、标签、搜索
config/development/      本地预览专用配置（把绝对地址指向 localhost）
themes/PaperMod/         主题（git submodule，不要直接改）
.github/workflows/       自动部署
```

## 本地预览

需要 Hugo Extended（`hugo version` 能打印出版本号即可）。在项目目录下执行：

```powershell
hugo server
```

浏览器打开 <http://localhost:1313/>。改动会自动刷新，`Ctrl+C` 停止。

注意：本地预览请用 `hugo server`，不要用 `hugo server --environment production`。前者会读取 `config/development/`，把 favicon 这类绝对地址指向本地；后者会指向线上域名，图标会加载不出来。

如果提示找不到 `hugo` 命令，说明当前终端还没读到新的 PATH，重开一个终端再试。

## 写一篇新文章

```powershell
hugo new content posts/my-new-post.md
```

然后编辑 `content/posts/my-new-post.md`，把开头 front matter 里的 `draft` 改成 `false`：

```toml
+++
title = "文章标题"
date = 2026-10-13
tags = ["CS"]
summary = "一句话摘要，会显示在列表页。"
+++
```

## 构建

```powershell
hugo --gc --minify
```

产物在 `public/`，这个目录不进版本库。

注意：`public/` 里的链接写死的是线上域名 `https://xinvista.github.io/`，所以不要用浏览器直接打开 `public/index.html` 预览——那样所有文章链接和图标都会指向一个还不存在的网站。看效果请用 `hugo server`。

本地预览的输出写在 `resources/_dev/`，不会覆盖 `public/`。

## 部署

推送到 `main` 分支即自动部署，由 `.github/workflows/deploy.yml` 完成。

首次部署前，需要在 GitHub 仓库里把 Pages 的来源设成 GitHub Actions：

**Settings → Pages → Build and deployment → Source → GitHub Actions**

之后每次 `git push`，一两分钟后站点就会更新。

## 主题

PaperMod 以 git submodule 方式引入，不要在 `themes/PaperMod/` 里直接改代码。所有自定义都放在 `assets/css/extended/`。

克隆仓库时要带上主题：

```powershell
git clone --recursive https://github.com/xinvista/xinvista.github.io.git
```

更新主题：

```powershell
git submodule update --remote themes/PaperMod
```

## 字体

- **Roboto**（正文拉丁字母）：SIL Open Font License 1.1
- **JetBrains Mono**（标题、代码、元信息）：SIL Open Font License 1.1

中文字形不使用 Web 字体，交给系统字体渲染（苹方 / 微软雅黑 / Noto Sans SC）。
