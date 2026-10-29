# feelime/distribution

Feelime 模型与词典数据的分发仓库：体积大的数据产物（词表、语音/手写模型）**不打进 APK**，从这里的 Release 附件按版本下载，app 端 sha256 校验后落盘挂载。

## 布局约定（参考 podic 词典包更新）

- 每个数据产物一个**固定 tag 频道**（tag 名不含版本号，如 `wubi86`），Release 永远持有当前最新版：产物更新时原地替换附件、同步更新 metainfo
- Release 附带一个 **metainfo json**（如 `wubi86-metainfo.json`），顶层即 `version` + `files[]`（file/sha256/size/url）+ notes；app 更新时**先下载 metainfo**，比对版本后再按 `files[].url` 逐个下载校验
- 下载支持 gh-proxy 加速（任意 `https://gh-proxy.com/` 前缀拼接 Release 附件 URL 即可）

## 许可

各 Release 附件的许可以其 notes 为准（如五笔词表衍生自 rime-frost，GPL-3.0）；Feelime 应用本体保持 MIT，不打包这些数据。
