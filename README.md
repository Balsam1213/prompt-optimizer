# prompt-optimizer 提示词优化器

把模糊、口语化的一句话需求，转化为**可直接复制投喂给任何 AI 的完整提示词**。

> Converts vague requirements into self-contained, copy-paste-ready prompts — as a portable [Agent Skill](https://agentskills.io) that works across models and agents.

## 它做什么

- **模糊需求 → 成品提示词**：你说「帮我写个提示词，让 AI 整理周报」，它产出一条自包含、角色/任务/约束/输出格式齐全的提示词，整块复制即可使用。
- **优化已有提示词**：粘贴现成的 prompt，它先诊断（目标不清、约束不可验收、缺输出格式、自相矛盾、无效空话……）再重写，并说明每处改动的理由。
- **图像 / 视频生成提示词**：基于六层约束模型（事实、媒介、结构、一致性、排除、参数），覆盖文生图 / 图生图 / 参考图反推、文生视频 / 图生视频 / 多镜头 / 路径控制、角色一致性（character sheet）、负面提示词，并按目标模型（Nano Banana、即梦、Midjourney、FLUX、Seedance、可灵、Veo、Sora）适配提示词形态。
- **不阻塞原则**：信息不全时基于合理假设直接产出，假设全部明示，最多留 3 个可选问题——回答与否都不影响当前产出可用。
- **一次到位**：你直接说「帮我生成一个视频：赛博朋克城市夜景」时，skill 在内部完成提示词优化，立刻交给会话可用的生成能力（生成工具 / MCP / 脚本 / 浏览器）出结果——一次对话搞定，不展示提示词、不落盘，除非你要求看。

## 安装

这是一个标准的 Agent Skill 文件夹（`SKILL.md` + `references/`），拷贝到对应目录即可：

| 工具 | 安装位置 |
|---|---|
| ZCode（全局） | `~/.agents/skills/prompt-optimizer/` |
| ZCode（单项目） | `<project>/.agents/skills/prompt-optimizer/` |
| Claude Code | `~/.claude/skills/prompt-optimizer/` |
| 其他支持 Agent Skills 的工具 | 该工具的 skills 目录 |

```bash
git clone https://github.com/Balsam1213/prompt-optimizer.git ~/.agents/skills/prompt-optimizer
```

**任何不支持 skill 的普通 AI**（网页版 ChatGPT 等）：把 [`SKILL.md`](SKILL.md) 的正文内容粘贴为对话第一条消息（相当于系统提示词），然后按它的工作流使用。

## 使用

装好后，只要你的话里带「提示词 / prompt」就会自动触发，例如：

- 「帮我写个提示词，让 AI 帮我写爬虫」
- 「优化这段 prompt：帮我写代码」
- 「我要给可灵写个视频提示词，一只猫在屋顶跑」

也可以用 `/prompt-optimizer` 强制加载。

要提示词本身时，输出固定为四段：**可直接投喂的提示词**（一个完整代码块）→ **关键假设** → **可选确认**（≤3 问，不答也没关系）→ **调整方向**。直接要生成结果时（「帮我生成一个视频：……」），一步出结果，提示词默认不展示，要求看时才给。

## 文件结构

```
prompt-optimizer/
├── SKILL.md                       # 双模式：提示词模式（五要素解析 → 撰写 → 固定格式交付）+ 生成模式（内部优化 → 直接投喂生成能力）
└── references/
    ├── frameworks.md               # 通用结构框架库：思维链 / few-shot / 输出模板锁定 / 自检清单
    ├── gen-core.md                 # 生成类核心：六层约束模型（事实/媒介/结构/一致性/排除/参数）+ 双语契约 + 复杂度分级
    ├── gen-image.md                # 生图：十二维维度、参考图反推、图生图编辑、失败模式
    ├── gen-video.md                # 视频：时间轴容量、POV 物理约束、路径控制、音频导演
    ├── gen-consistency-negative.md # 跨图跨镜一致性、角色工作表、负面词规范与词池
    └── gen-models-vocab.md         # 模型适配速查 + 中英词汇银行
```

## 可移植性

Skill 本体不依赖任何平台特定能力，全部工作用对话文本完成，产出的提示词在纯文本环境下可用。正文为中文，触发描述中英双语。

## 致谢

生成类提示词的六层约束模型、时间轴容量表、模型适配与词汇银行等方法论，深度整合自开源项目 [q253595422/prompt-constraints](https://github.com/q253595422/prompt-constraints)（MIT License），经裁剪适配本 skill 的双模式工作流。
