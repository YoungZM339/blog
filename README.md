# Blog

一个基于 Hugo 的中文博客源仓库，使用 diary 主题组织文章、页面和站点配置。仓库包含静态资源、自定义域名声明以及 GitHub Actions 发布工作流。

## 结构

- content/：文章、关于页和链接页等内容。
- themes/diary/：Hugo 主题。
- static/：静态资源与 CNAME。
- config.toml：站点主配置。
- even-config.toml：扩展配置。
- .github/workflows/：自动构建和发布工作流。

## 本地预览

安装 Hugo 后，在仓库根目录执行：

    hugo server -D

该命令会将草稿内容一并用于本地预览。

## 构建

生成静态站点：

    hugo -D

生成结果位于 public/。该目录是构建产物，不应与源内容混淆。

## 发布流程

当 master 分支收到推送时，GitHub Actions 会运行 Hugo 构建，并将 public/ 发布到 gh-page 分支。发布前请确认：

- GitHub Actions 部署密钥已经作为仓库秘密安全配置；
- CNAME 与域名托管设置一致；
- 新文章、外链、评论配置和图片都适合公开。

## 写作与维护

新增文章前请检查 front matter、发布日期、草稿状态和资源路径。不要在文章、配置或静态资源中提交密钥、私人联系信息或未获授权的素材。

## 许可证

请参阅仓库中的 LICENSE 文件。文章、图片和主题可能适用不同的版权或许可。