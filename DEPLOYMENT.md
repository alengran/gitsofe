# GitHub Pages 部署指南

## 🚀 完整部署流程

### 第一步：创建 GitHub 仓库

1. 登录 GitHub，点击右上角 "+" → "New repository"
2. 仓库名称建议：`software-site` 或 `yourname.github.io`
3. 选择 "Public"（公开仓库）
4. 不要勾选 "Add a README file"
5. 点击 "Create repository"

### 第二步：准备本地文件

1. **移动 Jekyll 文件到根目录**：
   ```bash
   # 将 html/ 目录下的所有文件移动到上级目录
   cp -r html/* .
   rm -rf html/
   ```

2. **确保目录结构正确**：
   ```
   .
   ├── _config.yml
   ├── _data/
   ├── _layouts/
   ├── _plugins/
   ├── assets/
   ├── index.md
   ├── Gemfile
   └── .gitignore
   ```

### 第三步：初始化 Git 仓库

```bash
# 初始化 Git 仓库
git init

# 添加所有文件
git add .

# 第一次提交
git commit -m "Initial commit: Jekyll software site"

# 添加远程仓库（替换为你的仓库地址）
git remote add origin https://github.com/你的用户名/仓库名称.git

# 推送到 GitHub
git push -u origin main
```

### 第四步：启用 GitHub Pages

1. 访问你刚创建的 GitHub 仓库
2. 点击 "Settings" (设置)
3. 在左侧菜单找到 "Pages"
4. 在 "Source" 下：
   - **Branch**: 选择 `main` 分支
   - **Folder**: 选择 `/root` 目录
5. 点击 "Save"

### 第五步：等待部署完成

- GitHub 会自动检测 Jekyll 项目并开始构建
- 大约需要 1-2 分钟
- 在 Pages 设置页面会显示部署状态
- 成功后会显示类似：`https://username.github.io/repository-name/`

## 🔧 如果构建失败

### 常见问题和解决方案

1. **Gemfile 锁文件问题**：
   ```bash
   # 更新 Gemfile.lock
   bundle update
   git add Gemfile.lock
   git commit -m "Update Gemfile.lock"
   git push
   ```

2. **插件兼容性**：
   编辑 `_config.yml`，确保只使用 GitHub Pages 支持的插件：
   ```yaml
   plugins:
     - jekyll-feed
     - jekyll-seo-tag
     - jekyll-sitemap
   ```

3. **删除不支持的功能**：
   暂时移除 `_plugins/` 目录，GitHub Pages 可能不支持自定义插件。

## 🌐 使用自定义域名

### 方法一：通过 GitHub 设置

1. 在仓库 Settings → Pages
2. 在 "Custom domain" 中输入你的域名
3. 在域名 DNS 设置中添加记录：
   ```
   CNAME yourusername.github.io
   或者
   A 记录指向 GitHub Pages IP
   ```

### 方法二：通过 CNAME 文件

1. 在项目根目录创建 `CNAME` 文件：
   ```
   www.yourdomain.com
   ```

2. 在域名注册商处设置 DNS 记录

## 📱 访问你的网站

部署成功后，可以通过以下地址访问：

- **仓库域名**: `https://username.github.io/repository-name/`
- **自定义域名**: `https://www.yourdomain.com/`（如果设置了）

## 🔄 自动部署设置

### GitHub Actions 自动部署

创建 `.github/workflows/jekyll.yml`：

```yaml
name: Build and Deploy Jekyll Site
on:
  push:
    branches: [ main ]
  workflow_dispatch: # 允许手动触发

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.1'
          bundler-cache: true

      - name: Build site
        run: bundle exec jekyll build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_site
```

## 🎯 验证部署

1. **检查构建状态**：
   - 访问仓库的 "Actions" 标签
   - 查看最新的构建是否成功

2. **测试网站功能**：
   - 访问部署的 URL
   - 测试搜索功能
   - 检查软件详情页面
   - 验证响应式设计

3. **SEO 检查**：
   - 查看页面源代码，确认 meta 标签正确
   - 测试社交分享功能

## 🔒 安全设置

### 访问控制

如果需要限制访问，可以：

1. **启用 HTTPS**：在 Pages 设置中启用
2. **设置环境保护**：在 Settings → Branches 中添加规则
3. **隐藏仓库**：设置为 Private（GitHub Pages 仍可公开访问）

## 📊 监控和维护

### 添加分析工具

在 `_layouts/default.html` 中添加：

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

### 定期更新

- 定期检查 Jekyll 和依赖版本
- 更新软件信息和下载链接
- 监控网站性能和可用性

## 🆘 部署问题排查

### 网站无法访问

1. 检查 Pages 设置页面是否有错误提示
2. 查看 Actions 标签的构建日志
3. 确认文件是否正确推送

### 样式丢失

1. 检查 CSS 文件路径
2. 确认 assets 目录正确上传
3. 清除浏览器缓存

### 功能异常

1. 检查浏览器控制台错误
2. 验证 JavaScript 文件是否正确加载
3. 确认数据文件格式正确

## 🎉 部署成功！

部署成功后，你将拥有：
- ✅ 公开可访问的软件推荐网站
- ✅ 自动 HTTPS 支持
- ✅ 自动化构建和部署
- ✅ 版本控制和回滚能力
- ✅ 免费托管服务

---

**恭喜！你的软件优选站现在已经上线，可以被全世界用户访问了！** 🚀