---
标题: codex-opencode-go-bridge-mac
类型: 实体
状态: 可用
tags:
  - AI
  - 工具
  - Codex
  - DeepSeek
来源:
  - 00-收集箱/Raw/2026-05-20-0635-codex-opencode-go-bridge-mac.md
创建时间: 2026-05-20
更新时间: 2026-05-20
摘要: Mac 平台本地桥接工具，让 Codex 一键接入 OpenCode Go 套餐，支持 DeepSeek V4 等最新模型，提供一键切换模型功能。
链接: https://github.com/Kellen223/codex-opencode-go-bridge-mac
---

# codex-opencode-go-bridge-mac

## 简介

Mac 平台的本地桥接工具，让 Codex CLI 一键接入 OpenCode Go 套餐，可使用包括 DeepSeek V4 在内的所有最新模型。

## 关键特征

- **一键接入**：直接把文档和 API 给 Codex 自动安装
- **模型切换**：内置一键切换模型功能
- **模型丰富**：支持 DeepSeek V4、V4 Flash 等最新模型
- **量大管饱**：V4 Flash 模型提供充足的使用额度
- **Mac 优先**：专为 Mac 平台优化

## 技术栈

- JavaScript (Node.js)
- 本地服务器模式 (server.mjs)
- MIT 开源协议

## 项目结构

```
codex-opencode-go-bridge-mac/
├── docs/           # 文档目录
├── mac/            # Mac 平台文件
├── server.mjs      # 本地服务器
├── package.json    # 项目配置
├── README.md       # 英文说明
└── README.zh-CN.md # 中文说明
```

## 使用场景

- 需要在 Codex 中使用 DeepSeek 系列模型
- 希望通过 OpenCode Go 套餐获得更多模型访问权限
- 需要快速切换不同 AI 模型的开发场景

## 相关笔记

- [[Codex]]
- [[DeepSeek]]
- [[OpenCode]]

## 待确认

- 具体安装步骤和配置细节
- 支持的模型完整列表
- 与 Windows 版本的差异

## 来源

- GitHub: https://github.com/Kellen223/codex-opencode-go-bridge-mac
- 00-收集箱/Raw/2026-05-20-0635-codex-opencode-go-bridge-mac.md
