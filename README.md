# 后记忆 — 博客源码

基于 [Hexo 3](https://hexo.io/) 的静态博客，主题 [yilia](https://github.com/litten/hexo-theme-yilia)，部署到 GitHub Pages。

- 线上地址：https://www.houjiyi.com/
- 部署仓库：`git@github.com:liwenliang/liwenliang.github.io.git`

## 环境要求

本项目依赖较老的 Hexo 3 生态，**请使用 Node 12**（见 `.nvmrc`）：

```bash
nvm install    # 首次安装
nvm use        # 切换到 v12.16.0
```

> 使用 Node 14+ 部署时可能报错 `TypeError: mode must be int32 or null/undefined`，切回 Node 12 即可。

## 首次安装

```bash
nvm use
npm install
```

## 常用命令

本项目建议统一使用 `npx hexo`（避免全局 `hexo` 命令找不到的问题）：

| 操作 | 命令 |
|------|------|
| 本地预览 | `npx hexo server` 或 `npx hexo s` |
| 新建文章 | `npx hexo new "文章标题"` |
| 生成静态页 | `npx hexo generate` 或 `npx hexo g` |
| 清理缓存 | `npx hexo clean` |
| 部署到 GitHub | `npx hexo deploy` 或 `npx hexo d` |

### 写文 → 发布（完整流程）

```bash
nvm use
npx hexo new "文章标题"          # 在 source/_posts/ 下生成 Markdown
# 编辑文章，填写 front-matter（title、date、categories、tags 等）
npx hexo clean && npx hexo g   # 生成静态文件到 public/
npx hexo d                     # 推送到 GitHub Pages
```

本地预览（不部署）：

```bash
npx hexo clean && npx hexo g && npx hexo s
# 浏览器打开 http://localhost:4000
```

## 目录说明

```
blog_source/
├── _config.yml          # 站点配置（标题、URL、部署等）
├── source/
│   ├── _posts/          # 博客文章（Markdown）
│   └── assets/          # 静态资源
├── themes/
│   └── yilia/           # 当前主题
│       └── _config.yml  # 主题配置
├── scaffolds/           # 新建文章/页面的模板
└── public/              # 生成的静态站点（git 已忽略）
```

## 配置修改

- **站点信息**（标题、域名、permalink）：编辑根目录 `_config.yml`
- **主题样式**（社交链接、菜单、侧边栏等）：编辑 `themes/yilia/_config.yml`
- **部署目标**：`_config.yml` 中的 `deploy` 段

## 常见问题

### `hexo: command not found`

全局 `hexo` 未在 PATH 中，改用 `npx hexo` 即可，例如 `npx hexo s`。

### 部署报错 `mode must be int32 or null/undefined`

Node 版本过高，执行 `nvm use` 切换到 Node 12 后重试。

### 部署报错 `in unpopulated submodule '.deploy_git'`

`.deploy_git` 被误登记为 git submodule，执行以下命令修复：

```bash
git rm --cached .deploy_git
rm -rf .deploy_git
echo ".deploy_git/" >> .gitignore
npx hexo d
```
