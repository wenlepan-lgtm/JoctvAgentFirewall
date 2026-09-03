# JoctvAgentFirewall — JOCTV 安全规则发布仓库

酒店运行端**只读**的安全规则全量快照发布库 (架构 §34.4)。本仓库只存无秘密发布物:

- `v1/` — `joctv.safety-feed.v1` 全量快照五件套: `manifest.json`
  (schema/sequence/version/SHA256/Ed25519 签名) + 规则/回复/正负例文件;
- `change-summaries/` — 每个版本的机器可读变更摘要
  (`joctv-safety-feed-change-summary-v1`, 由 joctv-safety-feed-skill 生成);

不存客户数据、凭据、工作站原始需求或任何模型权重。

## 当前发布

| 通道 | sequence | version | 发布时间 |
|---|---|---|---|
| v1 | 1 | 2026.08.18-1 | 2026-08-18T23:30:00+08:00 |

## 酒店端消费方式

后台「安全防火墙 → 平台安全规则库」每日 12:00 (Asia/Shanghai) 自动检查并原子应用:

```
Feed URL = https://raw.githubusercontent.com/wenlepan-lgtm/JoctvAgentFirewall/main/v1/manifest.json
```

校验链 = 授权窗口 → Manifest Schema → Ed25519 签名 (信任根为本机受控公钥,
**绝不来自本仓库**) → 文件 SHA256/大小 → 规则 Schema/受限正则 → 正负例回归
→ 原子应用; 任何失败保留 last-known-good, 不做增量拼接。

## 版本链与再生成

规则包只能在公司工作站用 `joctv-safety-feed-skill` (见内部 JoctvAgentAssets
仓库) 从上一完整版本源文件 + 机器可读变更请求生成, 再由冻结打包签名工具
构建后提交本仓库。直接改动本仓库文件会被酒店端签名校验拒绝。

## 许可证

Apache-2.0 (见 LICENSE)。规则内容为平台安全特征, 不含客户数据。
