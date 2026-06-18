# 🔐 安全策略 / Security Policy

## 支持版本 / Supported Versions

| 版本 | 安全修复 | 备注 |
|------|---------|------|
| 6.3.1-sec+ | ✅ | Mile 安全优化分支，默认关闭 headless 浏览器，启用时有安全确认弹窗 |
| <= 6.3.0 原版 | ⚠️ | `downloadEnhanced` 默认开启，无启用确认 |

## 安全设计 / Security Design

### 凭证保护 / Credential Protection
- API 凭证（Client ID + API Key）通过 Obsidian `SecretStorage` API 存储于系统钥匙串（Windows Credential Manager / macOS Keychain）
- 凭证**不会**以明文形式保存在 `data.json` 或任何配置文件中
- 支持"粘贴并解析凭证"一键填入

### 数据流向 / Data Flow
- **单向同步**：IMA 云端 → Obsidian 本地（不会上传本地笔记到任何地方）
- 网络请求仅发送至：
  - `https://ima.qq.com` — IMA OpenAPI（需要认证）
  - 知识库笔记中的外部 URL（提取网页内容时）
  - 知识库附件链接（下载图片/文件时）

### 下载增强的风险 / Download Enhancement Risks
- 开启后会使用 Electron `BrowserWindow` 启动**隐藏浏览器窗口**加载外部网页
- 该浏览器会执行目标网站的 JavaScript 代码
- 此版本（6.3.1-sec+）**默认关闭**此功能，开启时会弹出安全确认弹窗

## 依赖审计 / Dependency Audit

| 依赖 | 版本 | 来源 | 审计 |
|------|------|------|------|
| `defuddle` | ^0.18.1 | npm (官方包) | ✅ @kepano 维护，Obsidian 核心贡献者，8.1k+ stars |
| `obsidian` | latest | npm (官方包) | ✅ Obsidian 官方 API |

本分支**不使用**任何私有 fork 或第三方源，所有依赖均来自 npm 官方 registry。

## 报告漏洞 / Reporting a Vulnerability

如有安全问题，请在 [GitHub Issues](https://github.com/mile202206/obsidian-ima.copilot-sync/issues) 提出。

## 最佳安全实践 / Best Practices

1. ✅ 从 Obsidian 社区插件市场安装
2. ✅ 定期更换 IMA API Key（`https://ima.qq.com/agent-interface`）
3. ✅ 仅在需要微信文章完整提取时开启「下载增强」
4. ✅ 不要安装来源不明的 Obsidian 插件
5. ✅ 定期检查插件更新
