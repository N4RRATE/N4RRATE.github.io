# Bowen 的博客操作指南

方案：Hugo + PaperMod + GitHub Pages。文章在电脑上用 Markdown 写，推送到 GitHub 后自动发布。

## 先认识三个位置

- 本地文件夹：`/Users/huangbowen/Documents/bowen-blog`
- 本地预览：<http://localhost:1313/>（必须运行预览服务）
- 预定正式网址：<https://n4rrate.github.io/>（首次 GitHub 授权和部署完成后可访问）

## 第一次上手：修改已有草稿

1. 用 VS Code 打开整个 `bowen-blog` 文件夹。
2. 打开 `content/posts/my-first-note.md`。
3. 修改 `title`、`summary` 和正文，保存。
4. 终端进入博客文件夹后运行：

```sh
cd ~/Documents/bowen-blog
hugo server -D --disableFastRender --renderToMemory
```

打开 <http://localhost:1313/>，即可看到修改。服务已在运行时不用重复启动；按 Ctrl+C 停止。VS Code 也可以通过“终端 → 运行任务 → 博客：预览（包含草稿）”启动。

`+++` 之间是文章信息：

```toml
title = '我的标题'
date = '保留自动生成的日期'
draft = true
tags = ['C++', '算法']
summary = '一句话介绍'
```

- `draft = true`：草稿，仅带 `-D` 的本地预览展示。
- `draft = false`：正式构建会展示。日期在未来的文章要到那个时间才会展示。
- `+++` 下方才是正文。二级标题用 `##`。

注意：草稿不会显示在正式网站，但推送到公开 GitHub 仓库的草稿源码仍然公开，不适合存放私密笔记。

## 新建下一篇文章

在另一个终端运行（预览服务所在终端保持运行）：

```sh
cd ~/Documents/bowen-blog
hugo new content posts/series-review.md
```

编辑生成的 `content/posts/series-review.md`。文件名推荐简短英文，文章标题可以用中文。模板已经有“学习内容、理解、验证、疑问”四部分，不需要的删掉即可。

## 代码、公式和图片

代码用下面这样的围栏：

````markdown
```cpp
std::vector<int> nums{1, 2, 3};
```
````

行内公式：`\(O(n\log n)\)`。

独立公式：

```text
$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$
```

图片放到 `static/images/`（第一次自己新建 images 文件夹），正文使用：

```markdown
![图片说明](/images/example.png)
```

不要把本机绝对路径当图片网址；别人访问不到。

## 发布或更新文章

1. 检查预览，将要发布文章的 `draft` 改为 `false`。
2. 在终端运行构建，确认无错误：

```sh
hugo --gc --minify
```

3. 检查改动、保存版本、推送：

```sh
git status
git diff
git add content hugo.toml
git commit -m "更新学习笔记"
git push
```

上面仅加入文章和站点设置；如果新增了图片，还需要在提交前运行 `git add static/images`。没有修改时无需提交。

4. 查看 GitHub 仓库的 Actions 页面，等待“Build and deploy blog”显示成功，再刷新正式网址。一次推送会触发一次部署，通常需要几分钟；推送成功不等于部署成功。

## 常用文件

- `content/posts/`：文章。
- `content/posts/welcome.md`：代码与公式的功能示例，可以修改或删除。
- `hugo.toml`：站名、简介、菜单等设置。
- `archetypes/default.md`：新文章模板。
- `themes/PaperMod/`：主题，通常不用改。
- `.github/workflows/hugo.yaml`：自动构建和部署。
- `public/`、`resources/`：生成内容，已排除出 Git。

## 换电脑后恢复

先安装 Hugo 和 Git，然后运行：

```sh
git clone --recurse-submodules https://github.com/N4RRATE/N4RRATE.github.io.git
cd N4RRATE.github.io
hugo server -D --disableFastRender --renderToMemory
```

如果主题目录为空，在博客目录运行 `git submodule update --init --recursive`。

## 当前安装

- Hugo 0.168.0，本地与 GitHub Actions 使用相同版本。
- PaperMod 使用 Git 子模块固定到首次安装时的提交。
- 数学公式由 Hugo 在构建时生成原生 MathML，适用于现代 Safari、Chrome、Firefox，不依赖外部公式 CDN。
- 未配置评论、统计和自定义域名，之后确有需要再加。

参考：[Hugo 文档](https://gohugo.io/documentation/)、[GitHub Pages 部署](https://gohugo.io/host-and-deploy/deploy-to-github-pages/)、[PaperMod 功能](https://github.com/adityatelange/hugo-PaperMod/wiki/Features)、[公式渲染](https://gohugo.io/functions/transform/tomath/)。
