# solo-tables 文档站

**设计讨论稿；业务方案尚未实现。** 内容入口为 `content/docs/_index.md`。本目录是独立 Hugo 站点，不包含业务实现或业务部署。

## 固定依赖与官方来源

| 依赖 | 本站固定版本 | 官方依据 |
| --- | --- | --- |
| OINK Hugo Module | v1.5.1 | [OINK](https://github.com/pgsty/oink/tree/v1.5.1) |
| Hugo Extended | 0.168.0 | [Hugo 发布](https://github.com/gohugoio/hugo/releases/tag/v0.168.0) |
| Go | 1.27.3 | [官方 starter](https://github.com/pgsty/oink-starter) |

核验使用的 starter 提交为 `6662307ed544eefcfe7bd80a15bffbf3daedc147`；OINK v1.5.1 对应提交为 `ef3f1192250684b2d92a22a877e220e1ff6789f2`。模块版本与校验值记录在 `go.mod`、`go.sum`。主题通过模块下载，保持上游 Apache-2.0 许可证；没有复制或修改主题源文件。仓库原有许可证保持不变。

安装 Git、Go 1.27.3 和 Hugo **Extended** 0.168.0。OINK 使用内置 LibSass，无需 Node.js、npm 或外部 Sass。

## 本地构建与预览

从仓库根目录执行：

```bash
cd docs
go version
hugo version
GOWORK=off HUGO_MODULE_WORKSPACE=off go mod download github.com/pgsty/oink
GOWORK=off go mod verify
GOWORK=off HUGO_MODULE_WORKSPACE=off hugo --cleanDestinationDir --gc --minify --environment production --printPathWarnings
```

预览：

```bash
hugo server --baseURL http://localhost:1313/solo-tables/ --appendPort=false
```

打开 `http://localhost:1313/solo-tables/`。生产构建输出到 `docs/public/`；输出、缓存和下载包不进入 Git。首次模块解析需要网络。

保留告警以便检查。官方基线的 LibSass 弃用警告可接受；不要启用 `--panicOnWarning`。不使用 Hugo 的 `draft: true` 隐藏讨论稿，使用页面正文的显式状态提示。

## GitHub Pages

- `hugo.yaml` 的 canonical baseURL 为 `https://shyun-cc.github.io/solo-tables/`，不是已上线声明。
- 仓库根目录 `.github/workflows/docs-pages.yml` 在 PR 中只构建，不发布；只有 `main` 的推送或从 `main` 手动触发才能上传并部署文档。
- 合并后，仓库需已具备 Pages 使用权限，并在 Settings → Pages 将 Source 设为 GitHub Actions。工作流设置 `enablement: false`，不会自行启用 Pages 或改变仓库设置。本轮未执行这些设置操作。
- 仅部署作业持有 `pages: write` 与 `id-token: write`，构建作业只读；使用平台提供的临时身份，无需创建凭据。
- 合并前的草稿 PR 不代表公开站点已发布。若变更仓库名、所有者或使用自定义域名，应先更新 baseURL 并重新验证子路径。

## 评审与公开边界

请先回答 `content/docs/questions.md` 的 P0 问题，再将确认内容写回各页。没有明确结论的项目仍标记为提案或待确认。文档及 PR 只使用可公开内容和合成示例，不提交私人聊天、内部标识、秘密、凭据或用户财务信息。
