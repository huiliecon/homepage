# 个人主页：GitHub → Hugo → Netlify 部署流程

## 当前站点配置

- 源码仓库：https://github.com/huiliecon/homepage
- 正式分支：`master`
- Netlify 项目：`huili`
- 正式网站：https://huili.netlify.app/
- Hugo 版本：`0.69.2`（由 `netlify.toml` 指定）
- 主题：`academimal`，源码随仓库保存在 `themes/academimal/`
- 构建命令：`hugo --gc --minify`
- 发布目录：`public`

以上设置于 2026-09-27 在仓库和 Netlify 项目配置中核对。后续如调整分支、版本或构建规则，应同步更新本文。

## 日常更新哪些文件

| 内容 | 文件 |
|---|---|
| 首页介绍和精选论文 | `content/_index.md` |
| 研究成果与工作论文 | `data/research/research.yaml` |
| 研究页说明 | `content/research/_index.md` |
| 教学页面 | `content/teaching/_index.md` |
| 链接页面 | `content/link/_index.md` |
| 姓名、单位、主题等配置 | `config.toml` |
| 页面布局与样式 | `layouts/`、`themes/academimal/` |
| 静态附件 | `static/`（现有部分文件也保存在 `content/`，修改引用时先核对页面路径） |
| Netlify 构建规则 | `netlify.toml` |

## 自动发布流程

1. 在 GitHub 的分支上修改源文件并提交。
2. 发起目标为 `master` 的 Pull Request。
3. Netlify 接收到 GitHub 事件，拉取该提交，按 `netlify.toml` 安装指定 Hugo 并构建。
4. 预览构建执行 `hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL`，将生成的 `public/` 发布到独立的 Deploy Preview 地址。
5. 打开预览地址，检查首页、Research、Teaching、Links，以及简历和论文附件。
6. 确认修改后合并到 `master`。Netlify 自动执行生产构建，将成功构建的站点发布到 `https://huili.netlify.app/`。
7. 在 Netlify 的 Deploys 页面检查部署状态和日志。

GitHub 保存 Markdown、YAML、模板和图片等源码；Hugo 生成静态 HTML/CSS；Netlify 执行构建并托管生成的网站。日常使用这条 Git 部署链路，无需手动上传 `public/`。

## 本地预览（可选）

先安装与部署一致的 Hugo 版本，再执行：

```sh
git clone https://github.com/huiliecon/homepage.git
cd homepage
hugo version
hugo server
```

按终端给出的本地地址检查网页。一次性生产构建可运行：

```sh
HUGO_ENV=production HUGO_ENABLEGITINFO=true hugo --gc --minify
```

生成目录为 `public/`。这些命令应从仓库根目录运行。

## 常见检查

- Netlify 的生产分支为 `master`；不要误以为是 `main`。
- 独立分支推送目前不会生成 branch deploy；需要面向 `master` 的 Pull Request 才触发 Deploy Preview。
- YAML 缩进、Markdown front matter、主题模板语法错误可能导致构建失败，优先阅读部署日志中的第一条实际错误。
- 当前主题使用较旧的 Hugo 版本；升级前先在分支验证兼容性。
- GitHub Pages 仓库 `huiliecon.github.io` 不参与本 Netlify 站点的构建。旧附件链接需单独检查。
- 正式站点开启自动发布。对 `master` 的提交会触发真实发布，测试应先用 Pull Request 预览。

## 管理入口

- 部署列表：https://app.netlify.com/projects/huili/deploys
- 构建与分支设置：https://app.netlify.com/projects/huili/configuration/developer-settings
- 官方 Hugo 部署说明：https://docs.netlify.com/build/frameworks/framework-setup-guides/hugo/
