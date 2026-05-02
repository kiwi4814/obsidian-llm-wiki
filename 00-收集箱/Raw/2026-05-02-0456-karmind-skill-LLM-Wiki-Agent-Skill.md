---
tags:
  - LLM-Wiki
  - 知识管理
  - Agent-Skill
  - Obsidian
  - Karpathy
捕获时间: 2026-05-02T04:56
来源: GitHub - https://github.com/Lhy723/karmind-skill
---

# karmind-skill — Karpathy 风格 LLM Wiki Agent Skill

## 项目地址

https://github.com/Lhy723/karmind-skill

## 一句话概括

面向编程 Agent 的可移植 Skill，让 Agent 像维护代码库一样维护一个有引用、有链接、可持续更新的 Markdown 知识库。参考 Karpathy 的 LLM Wiki 理念。

## 核心理念

不要让 LLM 每次都从零检索碎片，而是持续把资料编译进结构化 wiki。每次新增资料、提问、发现矛盾，wiki 都变得更完整。

## 三层架构

- **Raw**（`raw/`）：不可变证据层，原始资料不重写
- **Wiki**（`wiki/`）：可维护知识层，source notes、实体页、概念页、综合分析页
- **Schema**（`AGENTS.md`）：操作手册，定义规则

## Wiki 内部结构

```
wiki/
├── index.md          # 内容导航
├── log.md            # 时间线日志（追加式）
├── sources/          # source notes + _drafts/（待复核草稿）
├── entities/         # 实体页
├── concepts/         # 概念页
├── questions/        # 问题页
├── synthesis/        # 综合分析页
├── cache/            # ingest-cache.json + assets-cache.json
├── assets/           # 镜像的图片/附件
└── reports/          # 体检报告、批处理报告
```

## 五大核心能力

1. **资料编译（Ingest）** — 从 raw 提取 claim、entity、concept、timeline、矛盾、开放问题，生成 source note
2. **缓存去重** — `ingest-cache.json` 跟踪 `pending → drafted → processed → failed → skipped`
3. **Wiki 体检（Doctor）** — 检测断链、孤儿页、缺失索引、过期内容、未编译资料
4. **批处理** — 支持外部 OpenAI 兼容模型批量生成待复核 draft，人工确认后才正式入库
5. **附件镜像** — 自动把 raw 引用的图片/附件复制到 `wiki/assets/`

## 附带脚本

- `init_wiki.py` — 初始化 wiki 目录结构，扫描已有笔记
- `ingest_cache.py` — 管理编译缓存（ensure/list/mark/reset）
- `mirror_assets.py` — 镜像图片和附件到 wiki/assets
- `model_batch_ingest.py` — 外部模型批量编译
- `wiki_doctor.py` — wiki 体检，输出报告
- `install.sh / install.ps1` — 一键安装脚本

## 支持的 Agent

Claude Code（plugin marketplace）、Codex、OpenCode、Trae、Skills CLI，以及通用 AGENTS.md 方式。

## 设计原则

- 原始资料不可变，wiki 页面可维护
- 每个重要结论都应追溯到 source note 或 raw source
- 矛盾和不确定性是一等公民，明确记录而非抹平
- 先用纯 Markdown + rg + 索引，规模大了再考虑向量数据库
- 输出不限于 prose，也可是表格、时间线、图表、slide、canvas

## 与现有方案的差异（待对比）

- karmind-skill 更标准化（通用 Agent Skill 格式），有 Python 脚本工具链
- 现有方案更定制化（Hermes Agent + Telegram 捕获 + 操作日志 + 热缓存）
- 可借鉴：ingest-cache.json 缓存跟踪、wiki_doctor.py 体检脚本、_drafts → 复核 → 正式入库工作流、批量模型编译
