# BuerYouth 个人博客（Hexo 版）搭建与维护说明

本说明文档将指导你如何使用 Hexo 构建、部署和维护一个美观的中文个人博客主页，并适配 GitHub Pages，适合 bueryouth.github.io 项目。

---

## 目录

1. 环境准备
2. 初始化 Hexo 博客
3. 主题安装与美化
4. 内容迁移与写作
5. 本地预览与调试
6. 部署到 GitHub Pages
7. 后续内容更新
8. 常见问题与参考

---

## 1. 环境准备

- 安装 [Node.js](https://nodejs.org/)（建议 LTS 版本）
- 安装 [Git](https://git-scm.com/)

安装 Hexo 命令行工具：

```bash
npm install -g hexo-cli
```

---

## 2. 初始化 Hexo 博客

建议在项目根目录新建 `hexo/` 目录：

```bash
mkdir hexo
cd hexo
hexo init
npm install
```

---

## 3. 主题安装与美化
：

- [Butterfly](https://butterfly.js.org/)

以 Butterfly 为例：

```bash
cd hexo
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

修改 `hexo/_config.yml`：

```yaml
theme: butterfly
```

主题详细配置请参考主题官方文档，可自定义头像、菜单、社交链接、首页轮播、文章卡片等。

---

## 4. 内容迁移与写作

- 将原有 Markdown 文档（如 docs/contents/ 下的 .md 文件）复制到 `hexo/source/_posts/` 目录。
- 每篇文章建议保留 YAML 头信息（title、date、tags、categories）。
- 新文章写作：在 `hexo/source/_posts/` 下新建 Markdown 文件即可。

---

## 5. 本地预览与调试

在 hexo 目录下运行：

```bash
hexo s
```

浏览器访问 [http://localhost:4000](http://localhost:4000) 预览博客效果。

---

## 6. 部署到 GitHub Pages

### 方案一：直接部署到 docs/ 目录

1. 修改 `hexo/_config.yml`，设置 `public_dir: ../docs`，生成静态文件到 docs/ 目录：

    ```yaml
    public_dir: ../docs
    ```

2. 生成静态文件：

    ```bash
    hexo clean
    hexo g
    ```

3. 提交并推送到 GitHub：

    ```bash
    git add .
    git commit -m "更新博客"
    git push
    ```

4. 确保 GitHub Pages 设置为 `docs/` 目录。

### 方案二：使用 hexo-deployer-git 自动部署

1. 安装部署插件：

    ```bash
    npm install hexo-deployer-git --save
    ```

2. 配置 `_config.yml`：

    ```yaml
    deploy:
      type: git
      repo: https://github.com/bueryouth/bueryouth.github.io.git
      branch: main
      message: "自动部署：更新博客"
      # 可选：指定发布目录
      # publish_dir: docs
    ```

3. 部署：

    ```bash
    hexo clean
    hexo g
    hexo d
    ```

---

## 7. 后续内容更新

1. 在 `hexo/source/_posts/` 新增或修改文章。
2. 本地预览无误后，执行：

    ```bash
    hexo clean
    hexo g
    hexo d
    ```

3. 推送代码到 GitHub，GitHub Pages 自动发布。

---

## 8. 常见问题与参考

- 主题文档：参见各主题官方文档，支持丰富自定义。
- Hexo 官方文档：[https://hexo.io/zh-cn/docs/](https://hexo.io/zh-cn/docs/)
- 部署问题可查阅 [Hexo 部署文档](https://hexo.io/zh-cn/docs/deployment.html)
- 如遇依赖问题，尝试 `npm install` 或升级 Node.js 版本。

---

## 目录结构建议

```
bueryouth.github.io/
├── hexo/                # Hexo 源码目录
│   ├── source/_posts/   # 博客文章
│   ├── themes/          # 主题
│   └── ...              # 其他 Hexo 文件
├── docs/                # Hexo 生成的静态页面（GitHub Pages 指向此目录）
│   └── ...              
├── BLOG-README.md       # 本说明文档
└── readme.md            # 项目说明
```

---

## 结语

本博客方案兼容 GitHub Pages，支持 Markdown 写作、主题美化、内容管理，适合个人技术博客长期维护。如需进一步美化或功能扩展，请参考主题文档或 Hexo 插件生态。
