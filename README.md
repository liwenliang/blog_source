# 后记忆 — 博客源码

基于 [Hexo 8](https://hexo.io/) 的静态博客，主题 [yilia](https://github.com/litten/hexo-theme-yilia)，部署到 GitHub Pages。

- 线上地址：https://www.houjiyi.com/
- 部署仓库：`git@github.com:liwenliang/liwenliang.github.io.git`

## 环境要求

Hexo 8 需要 **Node.js 20.19+**（见 `.nvmrc`）：

```bash
nvm install    # 首次安装
nvm use        # 切换到 v22.22.2
```

## 首次安装

```bash
nvm use
npm install
```

## 常用命令

推荐使用 `npm run` 或 `npx hexo`：

| 操作 | 命令 |
|------|------|
| 本地预览 | `npm run server` 或 `npx hexo s` |
| 新建文章 | `npx hexo new "文章标题"` |
| 生成静态页 | `npm run build` 或 `npx hexo g` |
| 清理缓存 | `npx hexo clean` |
| 部署到 GitHub | `npm run deploy` 或 `npx hexo d` |

### 写文 → 发布（完整流程）

```bash
nvm use
npx hexo new "文章标题"          # 在 source/_posts/ 下生成 Markdown
# 编辑文章，填写 front-matter（title、date、categories、tags 等）
npm run build                  # hexo clean && hexo generate
npm run deploy                 # 推送到 GitHub Pages
```

本地预览（不部署）：

```bash
npm run build && npm run server
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

全局 `hexo` 未在 PATH 中，改用 `npx hexo` 或 `npm run` 脚本即可。

### 运行 `hexo` 只显示 init / help / version

`package.json` 中缺少 `hexo` 字段会导致 Hexo 无法识别项目，需保留：

```json
"hexo": {
  "version": "8.1.2"
}
```

### 部署报错 `in unpopulated submodule '.deploy_git'`

`.deploy_git` 被误登记为 git submodule，执行以下命令修复：

```bash
git rm --cached .deploy_git
rm -rf .deploy_git
echo ".deploy_git/" >> .gitignore
npm run deploy
```

### 部署报错 `Permission denied (publickey)`

本机 GitHub SSH 密钥未配置，运行 `ssh -T git@github.com` 检查，或将 `deploy.repo` 改为 HTTPS 地址。
