# feelime/distribution

Feelime 模型与词典数据的分发仓库：体积大的数据产物（词表、语音/手写模型）**不打进 APK**，从这里按版本下载，app 端 sha256 校验后落盘挂载。

## 分发方案

### 频道模型

- 每个数据产物一个**固定 tag 频道**（tag 名不含版本号，如 `wubi86`）。该 tag 下只有一个 Release，**永远持有当前最新版**
- 产物更新 = 原地替换该 Release 的附件 + 同步更新 metainfo 里的 `version`。**不改 tag 名、不新开 Release**
- 仓库本体（git 内容）只放文档，**数据文件一律走 Release 附件**

### metainfo 规范

每个频道附一个 metainfo json（`<id>-metainfo.json`），作为 Release 附件与数据文件并列。**metainfo 里只允许相对路径**（`file` 是文件名，不含任何 URL）——换源/镜像只换 base，镜像站整目录照搬（metainfo + 产物文件）即可。

```json
{
  "id": "wubi86",
  "version": "1.0.0",
  "notes": "本版本说明",
  "files": [
    { "file": "wubi86.table.bin", "sha256": "<hex>", "size": 6974364 },
    { "file": "wubi86.prism.bin",  "sha256": "<hex>", "size": 1209660 }
  ]
}
```

### app 端更新流程

1. 下载 `{base}/<id>-metainfo.json`，读顶层 `version` 与本地已装版本比对；无更新结束
2. 有更新按「**base + `files[].file`**」逐个下载，`sha256` / `size` 校验后原子落盘挂载
3. 默认 base：`https://github.com/feelime/distribution/releases/download/<tag>/`

### 换源与加速

换源/镜像**只换 base**：镜像站整目录照搬 metainfo + 产物文件；gh-proxy 加速 = `https://gh-proxy.com/` 前缀拼接原 base。

## 许可

各 Release 附件的许可以其 notes 为准（如五笔词表衍生自 rime-frost，GPL-3.0）。Feelime 应用本体保持 MIT，不打包这些数据——GPL 义务只落在下载数据文件上。

发布/更新产物的操作规程见 [AGENTS.md](AGENTS.md)。
