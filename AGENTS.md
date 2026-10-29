# AGENTS.md — distribution 仓库操作规程

本仓库是 Feelime 数据产物（词表/模型）的发布渠道。**git 内容只放文档；一切数据文件走 Release 附件。**

## 发布一个新产物

1. 确定频道名 = 产物 id（如 `wubi86`、`asr-zh`），tag 名即频道名，**不含版本号**
2. 准备产物文件，逐个计算 sha256 与 size（`sha256sum` / `stat -c%s`）
3. 编写 metainfo：`<id>-metainfo.json`，顶层 `id` / `version` / `notes` / `files[]`；
   `files[].file` 只写**文件名（相对路径）**，禁止出现任何 URL——换源/镜像只换 base
4. 创建 Release：
   ```
   gh release create <频道名> --repo feelime/distribution \
       --title "<产物中文名>（最新版）" --notes "<中文 notes>" \
       <产物文件...> <id>-metainfo.json
   ```
5. Release notes（**中文**）必须包含：每个文件的 sha256/size 表格、数据来源与构建方式、**许可声明**（衍生数据写明上游与许可；GPL 数据须注明「可选下载，应用本体 MIT 不打包」）

Gitee 镜像（gitee.com/feelime/distribution）同步同一份 git 文档；Release 用 API 建
（token 走 oh-my-term secrets，不落仓库），附件接口**单文件上限 100MB**，超限文件
按上面的压缩规则处理。

## 更新一个已有频道

1. 构建新产物，核对 sha256/size
2. `version` 递增（semver 语义自定，保持只增不减），同步改 metainfo：
   `files[]` 换新清单、`notes` 写本版本说明
3. 原地替换：
   ```
   gh release delete-asset <频道名> <id>-metainfo.json -y
   gh release upload <频道名> --clobber <产物文件...> <id>-metainfo.json
   ```
4. 用 `gh release edit <频道名> --notes` 同步中文 notes（含新 sha256 表）
5. **不改 tag 名、不新开 Release、不覆盖历史语义**——app 靠 metainfo 的 `version` 判断更新

## 红线

- 数据文件**禁止 commit 进本仓库**
- metainfo **禁止出现绝对 URL**（只允许相对文件名；base 由 app 侧配置）
- 替换附件与更新 metainfo 必须同一批完成，不允许出现 metainfo 指向不存在附件的中间态
- 上游许可（如 GPL-3.0）必须在 Release notes 明示，并保持「应用本体 MIT 不打包数据」的表述
