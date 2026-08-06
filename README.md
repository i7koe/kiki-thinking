# kk^小記 博客部署指南

## 文件结构

```
blog/
├── _config.yml              # 站点配置
├── Gemfile                  # Ruby 依赖（本地开发用）
├── .gitignore
├── index.html               # 首页
├── about.md                 # 关于页
├── _posts/
│   └── 2025-08-06-hello-world.md   # 示例文章
├── _layouts/
│   ├── home.html            # 首页布局（含音乐播放器）
│   ├── post.html            # 文章布局（含评论区）
│   └── page.html            # 页面布局
├── _includes/
│   ├── footer.html          # 自定义页脚
│   ├── music.html           # 网易云音乐播放器
│   └── comments.html        # Giscus 评论
├── assets/
│   └── css/
│       └── style.scss       # 配色覆盖 + 自定义样式
└── README.md                # 本文件
```

---

## 部署到 GitHub Pages（5步完成）

### 第1步：创建 GitHub 仓库

1. 登录 [GitHub](https://github.com)
2. 点击右上角 **＋** → **新建仓库**（New repository）
3. 仓库名称填：`你的用户名.github.io`（例如你的用户名是 kk，就填 `kk.github.io`）
   - 这样部署后地址就是 `https://kk.github.io`
4. 「可见性」选 **公开**（Public）
5. 勾选 **添加 README 文件**（Add a README file）
6. 点 **创建仓库**（Create repository）

### 第2步：上传文件

将 `blog/` 目录下所有文件上传到仓库根目录。

> 注意：GitHub 网页上传不支持上传文件夹，但可以拖拽。如果觉得麻烦，推荐用方法二。

**方法一（网页上传）**：
1. 在仓库页面点 **添加文件**（Add file）→ **上传文件**（Upload files）
2. 把 `blog/` 目录里的所有文件和文件夹拖进去
3. 底部写个说明比如"初始化博客"，点 **提交更改**（Commit changes）

**方法二（Git 命令行，推荐）**：
```bash
cd blog/
git init
git add .
git commit -m "初始化博客"
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git branch -M main
git push -u origin main
```

### 第3步：开启 GitHub Pages

1. 进入仓库页面，点 **设置**（Settings，就是仓库页面最右边那个带齿轮图标的标签页）
2. 左侧菜单找到 **页面**（Pages）
3. 「构建和部署」（Build and deployment）下面：
   - 「源」（Source）选 **从分支部署**（Deploy from a branch）
   - 「分支」（Branch）选 `main`，右边选 `/(root)`
4. 点 **保存**（Save）
5. 等待 1-2 分钟，刷新页面，顶部会出现你的网站地址：`https://你的用户名.github.io`

### 第4步：配置 Giscus 评论

1. 回到仓库的 **设置** → 左侧 **常规选项**（General）
2. 滚动到「功能」（Features）区域，勾选 **讨论**（Discussions），点保存
3. 打开 [https://giscus.app](https://giscus.app)（中文界面）
4. 在「仓库」栏输入你的仓库名，格式：`用户名/用户名.github.io`（例如 `kk/kk.github.io`）
5. 往下翻到「映射」（Mapping），选 **pathname**
6. 「Discussion 分类」选 **Comments** 或 **Announcements**
7. 继续往下翻，页面底部会自动生成一段代码，找到这几行：
   ```
   data-repo="kk/kk.github.io"
   data-repo-id="R_kgDOXXXXXX"          ← 复制这个值
   data-category="Comments"
   data-category-id="DIC_kwDOXXXXXX"   ← 复制这个值
   ```
8. 回到你的 GitHub 仓库，打开 `_config.yml` 文件，点右上角铅笔图标编辑，替换这 4 个值：
   ```yaml
   giscus:
     repo: "kk/kk.github.io"              # ← 改成你的
     repo_id: "R_kgDOXXXXXX"              # ← 粘贴从 giscus.app 复制的
     category: "Comments"                 # ← 跟你在 giscus.app 选的一致
     category_id: "DIC_kwDOXXXXXX"        # ← 粘贴从 giscus.app 复制的
   ```
9. 点右上角 **提交更改**（Commit changes），GitHub Pages 会自动重新构建

### 第5步：发布第一篇文章

1. 在仓库的 `_posts/` 目录下新建文件，文件名格式：`YYYY-MM-DD-标题.md`
   - 例如：`2025-08-07-我的第一篇.md`
2. 文件内容这样写：
   ```markdown
   ---
   layout: post
   title: "我的第一篇文章"
   date: 2025-08-07
   ---

   正文内容用 Markdown 写作...
   ```
3. 提交更改，等 1 分钟，刷新网站就能看到新文章了

---

## 配色说明

| 元素 | 色值 | 修改位置 |
|------|------|---------|
| 页面背景 | #FFFFFF | `assets/css/style.scss` → `$background-color` |
| 正文字体 | #333333 | `assets/css/style.scss` → `$text-color` |
| 链接/按钮/标题悬停 | #007AFF | `assets/css/style.scss` → `$brand-color` |

改色方法：打开仓库里的 `assets/css/style.scss`，修改顶部那 3 行变量值，提交即可。

---

## 网易云音乐

当前嵌入的单曲 ID 为 `2001036566`。换歌方法：
1. 打开网易云音乐网页版，找到目标歌曲
2. 点击「生成外链播放器」
3. 复制链接里 `id=` 后面的数字
4. 打开仓库的 `_includes/music.html`，把 iframe 里 `src` 中的 ID 数字替换掉

---

## 本地预览（可选）

如已安装 Ruby 和 Bundler：
```bash
cd blog/
bundle install
bundle exec jekyll serve
```
浏览器打开 `http://localhost:4000`
