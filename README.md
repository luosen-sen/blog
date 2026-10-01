# 我的博客

用 **Hexo 7 + Butterfly 主题** 搭的个人博客，配好了 GitHub Pages 自动部署。
你只需要写 Markdown 文件，别的都不用管。

---

## 一、这个文件夹里都有什么

```
博客/
├── _config.yml             ← 网站主配置（标题、网址、分页…）
├── _config.butterfly.yml   ← 主题配置（头像、菜单、颜色…）
├── scaffolds/              ← 新建文章时套用的模板
├── source/
│   ├── _posts/             ← ★ 所有文章放这里，一个 .md 就是一篇文章
│   ├── about/index.md      ← 「关于」页面
│   ├── guestbook/index.md  ← 「留言」页面
│   ├── tags/index.md       ← 标签列表页（不要删）
│   ├── categories/index.md ← 分类列表页（不要删）
│   └── img/                ← ★ 所有图片放这里
├── .github/workflows/deploy.yml  ← 自动部署到 GitHub Pages 的配置
├── public/                 ← 自动生成的网页（别手动改，每次构建会覆盖）
└── node_modules/           ← 安装的依赖（不用动，会被 git 忽略）
```

⭐ 标记的两个目录是你日常唯一会碰的地方：**文章放 `source/_posts/`，图片放 `source/img/`**。

---

## 二、日常写文章（最常用）

先打开终端，`cd` 到这个文件夹，然后用 `npm` 脚本（不用记 hexo 的完整命令）：

```bash
npm new "文章标题"     # 1. 新建文章
npm run dev            # 2. 本地预览：浏览器打开 http://localhost:4000/blog/
npm run build          # 3. 生成最终网页
```

第 1 步会在 `source/_posts/` 下生成一个和标题同名的 `.md` 文件，
第 2 步启动本地服务器，**改完文件保存，页面会自动刷新**，`Ctrl+C` 停止。

没写完的文章可以先存草稿：

```bash
npm new draft "标题"        # 生成到 source/_drafts/，不会被发布
npm run dev -- --draft      # 本地预览草稿
npm run publish "标题"      # 写完了，移到 _posts 正式发布
```

### 文章格式

```markdown
---
title: 文章标题
date: 2026-10-01 20:00:00
tags:
  - 标签一
  - 标签二
categories:
  - 分类名
cover: /img/default_cover.svg
top: 1                    # 置顶，数字越大越靠前，可删掉
---

正文从这里开始写，支持标准 Markdown 语法。

<!-- more -->              ← 加这行，它前面的内容作为首页摘要
```

### 配图

图片统一放 `source/img/`，正文里这样引用：

```markdown
![图片说明](/img/我的图片.jpg)
```

- 换头像：把图片放进 `source/img/`，然后改 `_config.butterfly.yml` 里的 `avatar.img`
- 换默认封面：同样改 `_config.butterfly.yml` 里的 `cover.default_cover`

### 更多页面

```bash
npm new page "页面名"     # 新建独立页面，会生成 source/页面名/index.md
```

---

## 三、改网站外观

只改两个文件，都在根目录：

| 想改什么 | 改哪里 |
| --- | --- |
| 网站标题、副标题、作者、语言 | [_config.yml](_config.yml) 最上面的 `title` / `subtitle` / `author` |
| 顶部菜单、头像、社交链接、封面、评论开关 | [_config.butterfly.yml](_config.butterfly.yml) |
| 首页每页几篇文章 | [_config.yml](_config.yml) 的 `per_page` |

Butterfly 主题的配置项有几百个，完整中文文档：
<https://butterfly.js.org/posts/4aa8abbe/>

> 改完配置**一定要重新 `npm run build`** 才生效（`npm run dev` 模式下改配置要重启）。

---

## 四、部署到 GitHub Pages

仓库里已经写好自动部署脚本 [.github/workflows/deploy.yml](.github/workflows/deploy.yml)。
逻辑是：**每次 `git push` 到 `main` 分支，GitHub 就自动生成网页并发布**，本地不用管。

### 第一次配置（只做一次）

1. 去 <https://github.com/new> 建一个**空仓库**，名字用英文，比如 `blog`，**不要**勾选初始化 README。
   本站配置已按 `luosen-sen / blog` 写好；如果仓库名不叫 `blog`，记得同步改 [_config.yml](_config.yml) 里的 `root`。

2. 把代码推上去：

   ```bash
   git remote add origin https://github.com/luosen-sen/blog.git
   git push -u origin main
   ```

   第一次推送会**弹出浏览器窗口**，点 **Authorize** 授权即可（Git Credential Manager 会记住登录状态，以后不用再登）。

3. 打开仓库页面 → **Settings** → 左边 **Pages** → **Build and deployment / Source** 选 **GitHub Actions**。

等一两分钟，刷新页面，仓库的 **Actions** 标签页里显示绿色对勾就发布成功了。
地址是 <https://luosen-sen.github.io/blog/>。

> 特殊：如果仓库名就叫 `luosen-sen.github.io`，把 `root` 设成 `/`，网址就是 <https://luosen-sen.github.io/>。

### 以后更新

```bash
npm run dev      # 先本地看看
git add .
git commit -m "更新文章"
git push         # 推送后自动部署，几分钟后线上生效
```

---

## 五、常见问题

**改了文章但网站上没变化？**
```bash
npm run clean && npm run build
```
Hexo 有缓存，`clean` 清一下。线上没变就等两三分钟，或者去 Actions 看日志。

**本地预览地址是什么？**
`http://localhost:4000/blog/`，最后的 `/blog/` 就是 `_config.yml` 里 `root` 的值。
想直接访问 `http://localhost:4000/`，就把 `root` 改成 `/`（但这样线上资源路径也会变，上线前记得改回来）。

**端口被占用？** `npm run dev -- --port 4001` 换个端口。

**中文文件名的文章，网址是一串 %E4%BD%A0…** 正常现象，不影响访问。
想让网址好看，在 front-matter 里加 `abbrlink`（需要额外插件），或者用英文标题。

**代码块没有高亮？** 默认已开启。想让代码块带 Mac 风格窗口和复制按钮，见 `_config.butterfly.yml` 的 `code_blocks`。

**想加评论、访问量统计、RSS？** 全部在 `_config.butterfly.yml` 里开关，按 Butterfly 文档填配置即可。

---

## 六、技术栈

- [Hexo 7.3](https://hexo.io) —— 静态博客生成器
- [Butterfly 4.13](https://butterfly.js.org/) —— 主题
- Node.js ≥ 18（本地开发用）
- GitHub Actions + GitHub Pages —— 托管
