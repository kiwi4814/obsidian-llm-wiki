---
tags:
  - LLM-Wiki
  - Research-Skill-Graph
  - Obsidian
  - Karpathy
  - 知识管理
  - RAG
捕获时间: 2026-05-01 17:10
来源: 小黑盒
来源链接: https://api.xiaoheihe.cn/v3/bbs/app/api/web/share?h_camp=link&h_session_id=TxrFV9Woc5setgII&h_src=YXBwX3NoYXJl&link_id=550771cd8b9a
---

# LLM Wiki+Research Skill Graph+Obsidian 构建个人知识研究引擎

2026年4月3日，安德烈·卡帕西（OpenAI联合创始人、特斯拉前人工智能主管，也是"氛围编程"一词的创造者）发布了一条标题为"大语言模型知识库"的推文，讲述了他如今如何利用大语言模型构建个人知识维基，而非仅用于生成代码。

这条推文迅速爆红。 次日，他又发布了新内容：一份"创意文件"，完整阐述了该理念背后的架构、理念与工具。

所谓创意文件，也是他这次提出来的新概念。他的原话是：

> The idea of the idea file is that in this era of LLM agents, there is less of a point/need of sharing the specific code/app, you just share the idea, then the other person's agent customizes & builds it for your specific needs.

> "创意文件的理念是，在这个大语言模型智能体的时代，分享具体的代码或应用的意义/必要性已经不大了，你只需分享创意，对方的智能体就会根据你的具体需求进行定制并构建出来。"

这是一个微妙却意义深远的转变。传统上，开发者做出实用的东西后，会分享实现方案：一个 GitHub 代码仓库、npm 上的一个包、一个 Docker 镜像。

接收者会克隆它、配置它并运行它。

但在人人都能使用大模型智能体（Claude Code、OpenAI Codex、OpenCode、Cursor 等）的时代，分享创意可能比分享代码更有价值。

因为思路是可移植的，而代码是特定的。

卡帕西在 macOS 上使用 Obsidian 搭配 Claude Code。

你可能在 windows 上使用 VS Code 搭配 OpenAI Codex。

一个共享的 GitHub 代码仓库需要被复刻、修改和调试。

而一个共享的思路文件只需复制粘贴到你的智能体中，你的智能体就会构建出一个完全适配你具体环境的版本。

话说回来，LLM Wiki 解决了知识怎么存。

但是存进去了怎么用呢？

如果你只是基于这个知识库，给大模型一个提示词去问问题，那得到的答案可能也不是很好。

因为模型的上下文是有限的，注意力是会漂移的。

为了解决这个问题，我找到了一个和LLM Wiki完美互补的方案，Research Skill Graph。

Research Skill Graph 搭建了一套基于知识库做深度研究的方法论，它通过定义一套深度研究的方法论来规范LLM的研究行为，提升最终的效果。

这篇文章会完整系统的介绍LLM Wiki和Research Skill Graph的原理、使用方式以及如何将二者结合做出一个个人的知识库研究引擎。

## 1: RAG和传统笔记工具的共同的死因：维护成本太高

RAG系统和传统笔记工具都有一个共同的死因，那就是维护成本太高。

没有人愿意在周末花三个小时更新30个过时的Wiki页面。

人类对知识整理的热情曲线是这样的：前三天高涨，然后指数级衰减。

除了维护成本，RAG还有一个重要的问题，那就是知识无法沉淀。

RAG的工作流程大概是这样子的。

1. 输入一个prompt。
2. 程序对你的prompt进行分词
3. 分词后到向量数据库检索，每个检索的内容都有一个得分。
4. 程序根据得分进行rerank，筛选有效的信息。
5. 那个prompt+rerank出来的信息，再次调用LLM 进行内容生成。

这就是所谓的检索增强生成。

你有没有发现一个问题？

那就是RAG的流程，每次都是从1-5。跑完之后没有任何的反馈和沉淀。

Karpathy 在创意文件里面指出：

> "The LLM is rediscovering knowledge from scratch on every question. There's no accumulation."

> "LLM 每次都在从零重新发现知识。没有积累。"

这个问题的本质是认知浪费。

假设你有一个包含 50 份研究报告的知识库，想问一个需要综合其中 5 份报告的复杂问题。

在 RAG 模式下，LLM 必须找到这 5 份报告的相关片段，拼出答案。

下次你问一个相关但不同的问题，它又得重来一遍推理链。

RAG把知识当成一个可以随时搜索的数据库，却忽略了知识需要编译、链接、维护。

下面是传统RAG和LLM Wiki的对比。

- 知识处理时机：查询时（每次提问重新处理） → 摄入时（每条素材只处理一次）
- 交叉引用：每次查询临时发现 → 预构建并持续维护
- 矛盾检测：可能被忽略 → 摄入时标记
- 知识积累：无——每次从零开始 → 随素材和查询持续复利增长
- 输出格式：聊天记录（临时性） → 持久化 Markdown 文件（可复用）

## 2: LLM Wiki 是什么

2026 年 4 月 4 日，Karpathy 发布了那个 GitHub Gist。开头是这么写的：

> "A pattern for building personal knowledge bases using LLMs."

> "一个用 LLM 构建个人知识库的模式。"

Karpathy是这么描述LLM Wiki的，注意他用的词是 "pattern"（模式）。

LLM Wiki 的核心思想是：不要在查询时检索原始文档，而是让 LLM 持续地、增量地构建和维护一个持久的结构化的、相互链接的 Markdown 文件集合，也就是Wiki。

当你添加一条新素材时，LLM 不是简单地把它索引用于后续检索，而是阅读素材，提取关键信息，再整合到已有的 Wiki 中。

这个整合包括：更新实体页面、修改主题摘要、标注新旧数据之间的矛盾、加强或挑战正在演化的综合分析。

Wiki 是一个持久的、复利增长的产物。交叉引用已经存在。矛盾已经被标记。综合分析已经反映了你读过的所有内容。

## 3：LLM Wiki的三层架构

LLM Wiki 的架构由三层组成：

### 1. Raw 层 — 你的原始素材库

文章、论文、图片、数据文件——所有你搜集的原始素材。这一层是不可变的（immutable）。

LLM 永远不会动raw目录下的任何文件。

实际操作中，Karpathy 用 Obsidian Web Clipper 把网页剪藏为 Markdown，图片下载到 raw/assets/ 目录。

社区里还有一个值得注意的实践：vault 分离——保持个人主 vault 的高信噪比，另建一个独立 vault 承载 Agent 生成的内容（后文会详细讨论这个策略）。

### 2. Wiki 层 — LLM 编译后的结构化知识

Wiki 层是整个架构的核心，也是LLM的核心工作层。

每条新素材进来时，LLM 执行的是一次系统性的更新：阅读素材、写摘要页面、更新索引、更新相关的实体和概念页面、追加时间记录、检查矛盾。

一条素材可能会影响 10-15 个 Wiki 页面。

一个LLM Wiki典型的目录结构如下：

```
wiki/ 根目录下包含：
- index.md（主目录，所有页面的索引）
- log.md（时间线，所有操作记录）
- overview.md（高层综合）
- concepts/（概念页面）
- entities/（实体页面）
- sources/（素材摘要）
- comparisons/（比较分析）
```

index.md 是内容导向的总目录。每一页都被列出，附带链接、一句话摘要和可选的元数据（日期、来源数量等）。LLM 回答查询时，会先读 index.md 定位相关页面，再深入阅读。Karpathy 说这个机制在中等规模（约 100 条素材、数百个页面）下工作得惊人地好，完全不需要 embedding-based RAG 基础设施。

log.md 是时间线。只追加，不修改。每条记录以一致的格式开头（如 `## [2026-04-02] ingest | Article Title`），这样你可以用简单的 Unix 工具解析它——`grep "^## \[" log.md | tail -5` 就能获取最近 5 条记录。

### 3. Schema 层 — 规则和配置

schema层通过一个文档（比如 Claude Code 的 CLAUDE.md 或 Codex 的 AGENTS.md），告诉 LLM Wiki 怎么组织的、有哪些约定、摄入素材、回答问题和维护 Wiki 时该遵循什么工作流。

这个配置文件让 LLM 成为一个有纪律的 Wiki 维护者，而不是一个通用聊天机器人。

Schema 固化了三样东西：目录结构、页面约定、操作工作流。

页面约定里有一个关键的设计：每条笔记顶部的一行摘要。MindStudio 的搭建指南专门强调了这一点：

> "The one-line summary at the top of each note is surprisingly important. Claude reads it to decide whether the full note is relevant."

> "每条笔记顶部的一行摘要出乎意料地重要。Claude 靠它判断整条笔记是否相关。"

传统笔记是给人看的，我们自己记住什么东西在哪，然后导航过去。

LLM Wiki 的笔记是给模型看的，模型用摘要做初筛，只深入阅读相关页面。

每个 Wiki 页面都应有结构化的头部：标题、一行摘要、标签、创建日期、关联页面。

Schema并不是完全不可变的，如果用了一段时间后发现某些约定不好用，就改Schema。

### 编译类比

为了更好的理解这三层架构，我们可以用软件编译的流程来类比一下：

- raw是源代码，LLM是编译器，wiki是可执行文件，健康检查是测试套件，查询是运行时。
- 源代码（raw/）：原始的、人类编写的素材，不可变
- 编译器（LLM）：读取源代码，生成结构化输出
- 可执行文件（wiki/）：编译后的产物，可以直接使用
- 测试套件（health check / lint）：定期检查一致性、发现矛盾
- 运行时（query）：用户在编译后的知识上提问和探索

### Git 对象模型映射

还可以用Git的内部对象模型来映射知识结构：

- Blob · 原子知识单元 — 单个事实、模式、被否定的方案
- Tree · 目录/索引 — 分类和导航结构
- Commit · 来源事件 — 谁、什么时候、为什么加入这条知识
- Branch · 竞争假设 — 平行的研究线索
- Merge · 综合/收敛 — 假设的合成或解决
- Tag · 稳定快照 — 经过验证/审计的知识版本

这个映射的核心优势是内容去重——相同的知识产生相同的哈希值，相同的结论不会在 Wiki 里出现两次。

它同时解决了溯源问题：每条知识的来源、时间和理由都被记录在案。

## 4: LLM Wiki的维护

Wiki的维护主要靠操作：Ingest、Query、Lint

**Ingest（摄入）** 是 Wiki 增长的引擎。Karpathy 偏好一条一条摄入，保持参与感。

**Query（查询）** 不只是问问题。好的回答本身也应该被存回 Wiki，查询是双向建设——你消费知识，同时 Wiki 在增长。

**Lint（检查）** 是健康保障。健康检查主要做以下事情：

- 查找矛盾数据：扫描多篇文章中互相矛盾的事实和数字。这种事情眼睛扫不过来，但 LLM 可以在一次检查中找出"论文 A 说 X，论文 B 说非 X"这样的冲突。
- 补全数据空白：找到某篇文章引用了一个概念页面遗失的情况，自动去网上查找补全。相当于是让 Wiki 自我修复。
- 发现隐含关联：在表面看上去没有联系的页面之间找到隐含联系——这种事人类很容易忽略，LLM 却能擅长。
- 建议新页面：根据现有页面的覆盖范围 / 密度，推荐还应该新增哪些主题页面。

### 从手工编辑到工具化自动化：MCP Server

以上三个操作定义了 Wiki 的完整生命周期。开源项目 llmwiki 将 Wiki 的核心能力抽象为 7 个工具：

- wiki_query：做关键词搜索并返回页面内容
- wiki_search：对整个 wiki/ 文件夹做原始 grep
- wiki_list_sources：列出原始素材文件
- wiki_read_page：读取单个页面内容
- wiki_lint：检查孤儿页面和断裂链接
- wiki_sync：触发素材到 Wiki 的更新与转换
- wiki_export：导出为 AI 可理解的格式（llms.txt、JSON-LD 等）

这 7 个工具本质上就是把 Ingest、Query、Lint 三个手工操作变成了程序化接口。这样任何 Agent 都能直接操作你的 Wiki，llmwiki 目前已经适配了 Claude Code、Codex CLI、Cursor、Gemini CLI、Copilot覆盖了当前主流的 AI 编程助手。

### LLM Wiki 使用场景

LLM Wiki 可以用来做什么：

- **个人成长**：跟踪目标、健康、心理状态——把日记、文章、播客笔记归档，构建一个关于自己的结构化画像
- **深度研究**：在几周或几个月内深入研究一个主题——读论文、文章、报告，增量构建一个有演化论点的综合 Wiki
- **读书伴侣**：逐章归档，构建人物页面、主题页面、情节线索页面。Karpathy 提到了 Tolkien Gateway——一个由志愿者花几年时间构建的、包含数千个相互链接页面的粉丝 Wiki。你可以独自做到类似的事情

商业团队场景同样适用。由 LLM 维护的内部 Wiki，素材来源可以是 Slack 线程、会议转录、项目文档、客户通话记录。竞争分析、尽职调查、旅行规划、课程笔记——任何你在一段时间内积累知识并希望它有组织而非散乱的场景，LLM Wiki 都能帮忙。

### 社区争议

LLM Wiki 不是银弹，社区也有不同的声音。

nishchay7pixels 写道：

> "The knowledge stored could easily be corrupted and it will become impossible for user to figure that out. The more you rely on Agent the more you will start doubting your own memory."

> "存储的知识很容易被污染，而用户将无法辨别。你越依赖 Agent，就越会开始怀疑自己的记忆。"

这个担忧是合理的。gnusupport则认为 LLM Wiki 本质上是"一个自我延续的 LLM 上下文生成器"——没有 LLM，它只是一堆无组织的文件。

他主张回到 Doug Engelbart 的 Dynamic Knowledge Repository 理念，用 PostgreSQL 做确定性存储。他花了 23 年构建的 Hyperscope 系统积累了 245,377 名用户和 95,211 份超文档。

两个观点都有道理，但它们指向的是同一个问题：信任边界。

Karpathy 的 raw/ 不可变原则和 Obsidian CEO Steph Ango 的 vault 分离方案，都在做同一件事——给"什么是可信的"划一条清晰的边界。

保持个人主 vault 的高信噪比，另建一个独立的"Agent vault"承载 LLM 生成的全部内容。两套 vault 互不污染，你始终能在主 vault 里保持对知识来源的掌控。

devsarangi2 提出了另一个实际问题：团队扩展性。一个 PM 不需要 80% 的技术文档——同一份 Wiki 对不同角色的价值差异巨大。

也就是说，LLM Wiki 在个人场景下最有价值，团队场景需要额外的角色过滤层。

LLM Wiki 是一个强大的个人工具，但它有自己的适用边界。信任边界需要设计，角色差异需要处理，数据质量需要持续审计。

## Part 5: 从零搭建实战指南

理论讲够了，现在动手。以下是搭建一个 LLM Wiki 的完整步骤。

### 第 1 步：安装 Obsidian 和 Claude Code

Obsidian 是 LLM Wiki 的最佳前端：本地优先、图谱视图、插件生态齐全。

下载 Obsidian，创建一个新的 Vault——就是一个文件夹，里面所有东西都是纯 Markdown。

Claude Code 是目前非常适合 LLM Wiki 的 Agent。它可以直接访问本地文件系统。不需要复制粘贴，告诉它笔记在哪，直接问问题。

Claude Code能读取指定文件或整个目录、跨文件搜索、创建和更新笔记、执行 shell 命令来过滤和组织。

安装只需一行命令：

```bash
npm install -g @anthropic-ai/claude-code
```

### 第 2 步：创建目录结构

在 Vault 根目录下执行：

```bash
mkdir -p raw/sources raw/assets
mkdir -p wiki/index wiki/concepts wiki/entities wiki/sources wiki/comparisons
git init
```

这是最基础的骨架。raw/ 放原始素材，不可变；wiki/ 放 LLM 编译后的产物。

### 第 3 步：写 Schema 文件

Schema 文件是你的 CLAUDE.md（如果你用 Claude Code）或 AGENTS.md（如果你用 Codex）。它告诉 LLM 怎么维护这个 Wiki。以下是一个最小可用版本：

```markdown
# LLM Wiki Schema

## 项目结构
- raw/sources/ — 原始素材，不可变
- raw/assets/ — 图片等附件
- wiki/ — LLM 生成的 Wiki，由 LLM 维护

## 页面约定
每个 Wiki 页面必须包含 YAML frontmatter：
---
title: 页面标题
type: concept | entity | source | comparison
sources: [来源列表]
related: [相关页面]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

此外，每条笔记应包含一行摘要和标签，方便模型快速判断相关性：
Summary: 一句话描述这条笔记的核心内容
Tags: #topic1 #topic2

## 摄入工作流（Ingest）
1. 阅读 raw/sources/ 中的新素材
2. 在 wiki/sources/ 写一条摘要
3. 更新 wiki/index.md
4. 更新相关实体/概念页面
5. 检查矛盾并标记
6. 在 wiki/log.md 追加记录

## 查询工作流（Query）
1. 读取 wiki/index.md 定位相关页面
2. 阅读相关页面的完整内容
3. 综合回答，附引用
4. 将有价值的回答存回 Wiki
```

目录结构不要过度设计。一开始四五个顶层文件夹就够了，随着素材积累自然演化出子目录。过早规划层级，Schema 反而会变得脆弱。

### 第 4 步：摄入第一条素材

安装 Obsidian Web Clipper 浏览器扩展，把文章剪藏为 Markdown 保存到 raw/sources/。然后在 Vault 目录下启动 Claude Code：

```bash
cd ~/my-llm-wiki
claude
```

告诉它："请处理 raw/sources/ 中最新的素材。"它会按照 Schema 执行摄入工作流。

### 第 5 步：检查结果，迭代 Schema

打开 Obsidian，检查生成的 Wiki 页面。发现问题就改 Schema。Karpathy 建议的检验标准是10 条素材测试：摄入 10 条素材后，问一个需要综合多条素材的问题。如果 Wiki 给出了从单条素材中无法获得的洞察，系统就是有效的。

### 实战技巧

几条在搭建过程中会被反复验证的经验：

**保持术语一致。** 如果你在一些笔记里写"RAG"，在另一些笔记里写"Retrieval-Augmented Generation"，Claude 能关联它们——但选一种写法坚持用，结果会更干净。

**笔记要聚焦。** 一个一万字的杂烩文档，查询效果远不如十个一千字的专题笔记。每条笔记只覆盖一个主题，用 [[wikilinks]] 连接相关笔记。

**用 /inbox 模式处理粗糙素材。** 把不知道该如何分类的笔记先扔进 wiki/inbox/，堆到一定数量再让 Claude 帮你归档整理。这比每条笔记都精雕细琢效率高太多。

**充分利用 Obsidian 插件生态。** Karpathy 在 Gist 里推荐了几个插件：
- **Marp**：基于 Markdown 的幻灯片格式，Obsidian 也有对应插件——你可以直接从 Wiki 内容导出演示文稿，不需要 PowerPoint。
- **Dataview**：能跑查询页面 frontmatter——只要你的 LLM 给每个 Wiki 页面都按俱乐部约定添加了标签、日期和来源计数等 YAML 格式元数据，Dataview 就能自动生成动态表格和列表。例如只要写一条 Dataview 查询语句，就可以列出"过去七天内所有更新过的概念页面"，比手动维护索引高效太多了。

**图片本地化。** Karpathy 在 Gist 里分享了个具体实践，把图片也下载到本地（而不是仅保留外部 URL 引用）。在 Obsidian 的 Setting → Files and links 设置附件文件夹的路径是一个固定目录（比如 raw/assets/），搜索 Hotkeys 里的"Download"，把"Download attachments for current file"绑定一个快捷键（比如 Ctrl+Shift+D）。粘进文章后按下快捷键，所有图片都会自动下载到本地。这样你的 LLM 不但可以看到图片，还可以直接引用到本地文件。

### 进阶工具

当 Wiki 足够大（超过 50 条各类素材）你可能会发现搜索上有些力不从心。有以下三条路可以走：

**强化搜索能力**：除了 Obsidian 自带的功能，Karpathy 还推荐了一个开源项目：qmd——Shopify CEO Tobi Lutke 构建的原生 Markdown 搜索引擎，结合了 BM25/向量搜索 + LLM 重排序。更大规模（上百条以上），可以用 LlamaIndex 自行在 Markdown 文件构建向量索引，为 Claude 加强语义搜索能力。

**端到端工具化**：如果你不想手动设置 Schema 和 Wiki 目录结构，Pratiyush 推出了一个开箱即用的解决方案 llmwiki。除了自动化，它的核心价值还在于解决了 LLM Wiki 模式的一个实际痛点：你用不同 Agent（Claude Code/Cursor/Codex）进行的会话记录，可能分别被分散保存在不同目录下面。llmwiki 自动收集你的 Agent（目前支持 Claude Code 和 Cursor）产生的所有 .jsonl 转录文件，并转换为 Karpathy 规范的 Wiki 格式。

此外还自带静态站点生成器：全局搜索（Cmd+K），暗色模式，代码语法高亮，面包屑导航，相关文章推荐，阅读时长估算……把本来只是一个文件夹的 Wiki 变成了一个可浏览的知识站点。更重要的是它的"双格式输出"设计：每个生成的 HTML 页面对应一个 .txt 的纯文本版本和 .json 文件，以及站点级的 llms.txt 和 JSON-LD 图谱，从而让其他 AI Agent 能直接消费你的 Wiki 内容。

**自动化工作流程**：llmwiki 还提供了 SessionStart 的钩子函数——让你的 Wiki 在每次启动 Claude Code 时自动同步新会话，还有文件观测器后台轮询 Agent 的存储目录。换句话说，llmwiki 让 Wiki 的生长过程可以完全被动：你正常使用 Claude Code / Cursor / Codex 处理知识，而 Wiki 在背景自动更新编译。

## 6: 从知识存储到知识使用 — Research Skill Graph

话说回来，LLM Wiki 解决了"知识怎么存"。

但是存进去了怎么用呢？

如果你只是基于知识库，给大模型一个 prompt 去问问题，那跟直接问豆包区别不大。垂直领域的知识还好说，科普类的素材，你的 Wiki 未必拼得过通用模型的知识储备。

真正的问题出在深度研究上。LLM Wiki 可能有一肚子货，但不知道怎么吐出来。

背后是两个硬伤。

**第一个硬伤：上下文窗口塞不下。** 每个模型的推理能力都随输入长度增加而下降。LLM 对上下文的开头和结尾表现还行，中间部分的注意力会显著丢失。简单点说，你塞进去的领域知识越多，Agent 对这些知识的处理能力就越差。

**第二个硬伤：研究方法论缺失。** 人类做研究有一套方法论，AI 做研究通常就是一个 prompt 搞定。方法论层面缺了一整层，AI 研究的产出自然浅薄。

这两个问题引出了另一个创意文件：Research Skill Graph。

### 6.1 Research Skill Graph 的设计理念

Skill Graph 的核心理念不是某个具体的文件夹结构或视角组合，而是四条设计原则。

**原则 1：方法论固化。** 研究方法论不应该存在于 prompt 里，它应该写进文件。Prompt 是一次性的，文件是持久的。你把"怎么看问题""怎么评估来源""怎么处理矛盾"写成独立的 .md 文件，Agent 每次都用同一套方法论，结果是可复现的。

**原则 2：图谱而非容器。** 知识库是一个容器，你往里扔东西，需要的时候搜索。Skill Graph 是一张图谱，每个知识节点通过 [[wikilinks]] 互连，Agent 像研究员查文献一样按需导航，而不是把所有内容一次性塞进上下文。

这直接回应了"Lost in the Middle"问题：每个节点足够短，没有中间可以迷失。

**原则 3：强制多角度，拒绝单一结论。** 拿到一个研究问题后，Agent 必须从多个完全不同的角度重新思考，拒绝直接给出答案。常见视角比如：技术、经济、历史、地缘、反向、第一性原理。

我们可以根据自己的领域调整。做伦理研究可能需要加一个"利益相关者"视角，做市场分析可能需要加一个"竞争格局"视角。

方法论的核心不变：强制多角度，视角之间的张力才是洞察的来源。

> "Each lens must RETHINK the question, not just add more information. The technical lens and the contrarian lens should feel like they were written by two different researchers who disagree with each other."

> "每个视角必须重新思考问题，而不是简单叠加信息。技术视角和反向视角读起来应该像是两个互相不同意的研究员写的。" —— The Smart Ape（AI 研究方法论博客）

**原则 4：质量可审计。** 研究产出必须可以事后追问。"这个结论基于什么等级的来源？为什么技术视角和反向视角冲突了？"这要求来源分级、合成规则、矛盾处理协议都写成文件，每个环节有文档记录。

### Research Skill Graph 目录结构

下面是一个 Research Skill Graph 的具体实现：

```
research-skill-graph/ 根目录下包含：
- index.md（方法论固化：Agent 从这里启动，读执行流程）
- research-log.md（质量可审计：跨项目日志，每个环节可追溯）
- methodology/（方法论固化：4 个文件定义研究方法论）
  - research-frameworks.md / source-evaluation.md
  - synthesis-rules.md / contradiction-protocol.md
- lenses/（强制多角度：每个文件定义一个视角的行为约束）
  - technical.md / economic.md / historical.md / geopolitical.md
  - contrarian.md / first-principles.md
- projects/（按研究课题归档）
- sources/（来源记录模板）
- knowledge/（图谱而非容器：概念和数据点跨项目累积）
  - concepts.md / data-points.md
```

### 6.2 质量保障机制

Research Skill Graph 使用5种层次来评估来源：

- **Tier 1 · 原始数据** — UN 数据集、同行评审论文
- **Tier 2 · 权威分析** — 官方技术博客、论文
- **Tier 3 · 专业评论** — 知名技术博主、行业报告
- **Tier 4 · 社区讨论** — Hacker News、Reddit 技术帖
- **Tier 5 · 社交轶事** — Twitter 帖子串、未验证信息

这套来源评估标准也有配套红旗降级规则。如果某个来源存在利益冲突、混淆因果性或使用情绪化的语言，Agent 会自动给这个来源降级。

当进行跨视角合成时，至少需要 4 个视角同时指向同一方向，才能形成高置信度的结论。单个视角之间的矛盾并不会被解决，而是会被记录下来。

这套系统最大的价值在于：可复现性。对于同一个来源，LLM 今天可能认为它是"高可信"，明天又可能会因为另一些"证据"而跳过。而如果把评估标准写进文件，Agent 就会用同一个尺子次次都对标。

### 6.3 Research Skill Graph 的局限性和适用边界

说完优势，Research Skill Graph 还有三个需要你注意的局限性。

**该系统的天花板取决于你写视角的文件质量。** 效果上限 = 视角文件写得有多严谨。你如果在反向视角文件里面，写的核心问题跟技术视角文件差不多，那 Agent 产出出来的所谓"反向分析"，除了用来反向的角度之外，跟技术分析没有区别。

**错误会越积累越多。** 如果 Agent 在某个项目里写了一个错误的数据点到 data-points.md，下次项目恢复性时会继承过去项目的 data-points.md。这个问题跟维基百科面临的"错误积累"问题是一样的：越早期的错误会越致命，因为其后所有内容都是用它建立的。

**没有银弹。** 此方法最适合于：（1）需要深度思考的分析场景；（2）时间较充裕的项目（几天到几周的时间量级）；（3）需要可审计性的使用场景，例如储蓄调查、政策分析或者投资决策支持。不适合一二类问题：对答机器人（"这个 bug 怎么修"）、百度分答（"vLLM 最新版本是多少"）或创意写作（写小说、设计 Logo）。

## 7: 1+1>2 — LLM Wiki x Research Skill Graph 实战组合

LLM Wiki 回答的是"我知道什么"。Research Skill Graph 回答的是"我怎么研究"。

两个系统在技术栈上天然兼容。都用纯 Markdown 文件，都用 [[wikilinks]] 做节点互连，都在 Obsidian 里运行，都能被 Claude Code 直接读写。

不需要任何适配层。放进同一个 Vault 就能用。

### 7.1 组合方案实战：一个研究课题的完整旅程

下面用一个具体的研究课题走一遍完整流程。课题："LLM 推理成本优化"。

**第一步：LLM Wiki 编译知识。** 把 30 篇相关素材拖进 raw/。论文（vLLM、TGI 的技术报告）、技术博客（Hugging Face 的推理优化系列）、benchmark 报告（Speculative Decoding 的延迟对比数据）。

每条素材逐一 ingest。LLM 编译出实体页面（[[vLLM]]、[[Tensor Parallelism]]、[[Speculative Decoding]]）、概念页面（[[KV Cache 优化]]、[[量化方法]]）、比较页面（[[vLLM vs TGI]]）。

矛盾被自动标记。论文 A 说 INT4 量化几乎无损，论文 B 在长文本场景下测出显著质量下降，这个冲突会被写进相关页面的"contradictions"段。

一条素材更新 10-15 个页面。30 条素材跑完，你的 Wiki 里已经有了一张推理优化领域的知识网络，有实体、有概念、有冲突、有时间线。

**第二步：Skill Graph 六视角分析。** 在 research-skill-graph/index.md 中填入研究问题。Agent 启动研究流程，先读 index.md，再读方法论文件，然后逐视角分析。

连接点在 knowledge/ 目录。Agent 做六视角分析时，会直接引用 Wiki 上已经编译好的页面，而不需要重新检索。六个视角每个都有单独的路线：

- **Technical 视角**：引用 [[KV Cache 优化]] 的 benchmark 结果和 [[vLLM vs TGI]] 的对比实验结果。这些都是经过验证、Tier 1/Tier 2 源头整理编译的产物。
- **Economic 视角**：整合不同方案的硬件投入和 TCO 数据。节省下来的推理算力去哪了？换来了更多的部署复杂度？这些 Tradeoff 要通过金钱来衡量。
- **Historical 视角**：梳理推理优化技术是如何从贪心解码 → Speculative Decoding → MoE 演进的。每一步的优化解决了什么问题，又带来了什么新问题。
- **Contrarian 视角**：对刚刚整理到的结论提出质疑。假设"成本优化是无价的"的前提是不对的。在特定场景下，优化带来的延迟抖动和工程复杂度，可能让你比裸加 GPU 还要亏钱。
- **First Principles 视角**：归 Zero。推理就是矩阵乘法运算，由于矩阵不能缓存，所以成本被内存带宽锁定。以此为底线，哪些优化算子是根本，哪些只是在浪费运算资源在边界"优化"？

**第三步：合成闭环。** 六视角分析完成后，关键发现被写回 Wiki。比如"Speculative Decoding 在长文本场景下的实际 ROI 低于理论值"，这个发现变成 Wiki 的新页面或已有页面的更新段落。下次 ingest 新素材时，这些已编译的洞察参与更新。

正循环就这样转起来了：Wiki 提供持续积累的知识燃料，Skill Graph 用方法论引擎精炼出洞察，洞察写回 Wiki 成为新的已编译知识。每一个循环都让两个系统同时变得更厚。

## 总结

文章说了两件事：知识如何存储（LLM Wiki），知识如何运用（Research Skill Graph）。

存和用是同一个飞轮的两面。只存不用，Wiki 变成数字垃圾场。只用不存，每次研究从零开始。

Karpathy 在推文里说了一句话：

> "The LLM is rediscovering knowledge from scratch on every question. There's no accumulation."

> "LLM 每次都在从零重新发现知识。没有积累。"

这句话不只是 RAG 的问题。它也是每一个用 AI 做研究的人面临的问题。

当今时代知识的获取重来不是难事，如何让知识被用起来是关键。

搭一个出来LLM Wiki + 研究引擎，两小时就能跑通第一条素材。关键不是搭得多完美，是先让它转起来。

---

来源：小黑盒
原文链接：https://api.xiaoheihe.cn/v3/bbs/app/api/web/share?h_camp=link&h_session_id=TxrFV9Woc5setgII&h_src=YXBwX3NoYXJl&link_id=550771cd8b9a
