---
tags: []
捕获时间: 2026-04-30
来源: 小黑盒 - 4月GitHub Trending月榜
---

4月的 GitHub Trending 月榜数据来了，有点神仙打架的意思。

19 个项目一共获得了 46.7 万 Star。榜单里面 andrej-karpathy-skills 凭借一个CLAUDE.md 文件获得了 8.9 万 Star 。Hermes Agent 本月增长了 10.8 万 Star，总 Star 数已经突破 12.4 万。microsoft/markitdown 本月增长了 2.6 万，claude-mem 本月增长了 2.8 万，rtk 本月增长了 2.3 万……。

榜单的19个项目中，至少有10个都是为 AI 编程助手（Claude Code / Codex / Cursor 等等）服务的。整个软件开发范式已经发生了变化。

今天把排行榜按AI 编程助手增强、Agent 基础设施、专业领域 / 实用工具这三个类型展开聊聊。

## AI 编程助手增强

### [andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) — 98,136 Stars / 月增 88,829
一个 CLAUDE.md 文件，Karpathy 将他对LLM 编程的经验，提炼成四条最佳实践原则。

### [claude-mem](https://github.com/thedotmack/claude-mem) — 69,494 Stars / 月增 27,718
Claude Code 的长期记忆插件。三阶段：捕获 → 压缩 → 注入。渐进式披露设计控制 token 消耗。

### [claude-howto](https://github.com/luongnv89/claude-howto) — 30,197 Stars / 月增 26,211
"周末入门 Claude Code"的教程文档。结构化、可视化、示例驱动的指南。

### [free-claude-code](https://github.com/Alishahryar1/free-claude-code) — 18,019 Stars / 月增 15,944
代理服务器，将 Claude Code API 请求路由到免费/低成本的模型提供商。

### [oh-my-codex](https://github.com/Yeachan-Heo/oh-my-codex) — 26,752 Stars / 月增 23,981
OpenAI Codex CLI 上的多智能体编排框架。四个内置 Skill。

### [rtk](https://github.com/rtk-ai/rtk) — 38,082 Stars / 月增 23,044
命令输出压缩工具，减少 60-90% token 消耗。Rust 实现，单二进制。

### [mattpocock/skills](https://github.com/mattpocock/skills) — 40,781 Stars / 月增 24,449
Total TypeScript 作者整理的 Claude Code Skill 集合，解决四大常见痛点。

## Agent 基础设施

### [Hermes Agent](https://github.com/NousResearch/hermes-agent) — 124,014 Stars / 月增 107,890
本月绝对统治者。作者说自己从 OpenClaw 迁移到了 Hermes，两者可互补。

### [Archon](https://github.com/coleam00/Archon) — 20,094 Stars / 月增 6,309
AI 编码智能体工作流引擎。YAML 定义开发流程，像 Dockerfile 标准化了基础设施。

### [GenericAgent](https://github.com/lsdefine/GenericAgent) — 8,130 Stars / 月增 7,182
极简自进化 Agent 框架。~3K 行代码，9 个原子工具。设计哲学：不预设技能，靠进化获得能力。

## 专业领域 / 实用工具

### [Kronos](https://github.com/shiyu-coder/Kronos) — 21,944 Stars / 月增 10,628
首个开源金融市场 K 线基础模型。45+ 交易所数据预训练。AAAI 2026。

### [FinceptTerminal](https://github.com/Fincept-Corporation/FinceptTerminal) — 17,521 Stars / 月增 14,039
金融智能终端，宣称取代 Bloomberg Terminal。

### [markitdown](https://github.com/microsoft/markitdown) — 118,530 Stars / 月增 25,993
微软出品，各种文件格式转 Markdown。AI 全栈时代的格式管道。

### [OpenScreen](https://github.com/siddharthvaddem/openscreen) — 33,554 Stars / 月增 24,564
Screen Studio 免费开源替代。PPT 高质量演示视频。

### [PPT Master](https://github.com/hugohe3/ppt-master) — 9,315 Stars / 月增 5,881
AI 文档转原生可编辑 PPTX。

### 其他
- Google AI Edge Gallery (22,250 / +6,809)：端侧 ML 展示
- Google LiteRT-LM (4,494 / +3,481)：端侧 LLM 推理引擎
- hackingtool (68,270 / +12,549)：安全工具全家桶
- DeepTutor (22,493 / +11,653)：Agent-Native 个性化学习助手

## 总结
1. AI 编程助手进入"生态系统"阶段
2. 减少 Token 成本是巨大需求
3. 垂直领域小模型开始显现价值
