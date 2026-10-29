# feelime/distribution

Feelime 模型与词典数据的分发仓库：体积大的数据产物（词表、语音/手写模型）**不打进 APK**，从这里的 Release 附件按版本下载，app 端 sha256 校验后落盘挂载。

## 布局约定

- 每个 Release = 一份数据产物集，tag 形如 `<产物名>-<版本>`（如 `wubi86-1.0.0`）
- 附件为产物文件本体；Release notes 记录来源、sha256 与许可
- 下载支持 gh-proxy 加速（任意 `https://gh-proxy.com/` 前缀拼接 Release 附件 URL 即可）

## 许可

各 Release 附件的许可以其 notes 为准（如五笔词表衍生自 rime-frost，GPL-3.0）；Feelime 应用本体保持 MIT，不打包这些数据。
