# prompt-optimizer 提示词优化器

把模糊、口语化的一句话需求，转化为**可直接复制投喂给任何 AI 的完整提示词**。

> Converts vague requirements into self-contained, copy-paste-ready prompts — as a portable [Agent Skill](https://agentskills.io) that works across models and agents.

## 它做什么

- **模糊需求 → 成品提示词**：你说「帮我写个提示词，让 AI 整理周报」，它产出一条自包含、角色/任务/约束/输出格式齐全的提示词，整块复制即可使用。
- **优化已有提示词**：粘贴现成的 prompt，它先诊断（目标不清、约束不可验收、缺输出格式、自相矛盾、无效空话……）再重写，并说明每处改动的理由。
- **图像 / 视频生成提示词**：为 Sora、Veo、可灵、即梦、Midjourney 等工具撰写画面描述类提示词（七要素模板 + 运镜词汇表）。
- **不阻塞原则**：信息不全时基于合理假设直接产出，假设全部明示，最多留 3 个可选问题——回答与否都不影响当前产出可用。
- **提示词库，免粘贴复用**：优化结果自动存入工作区 `prompts/` 目录。之后你说「生成视频 / 出片」，agent 直接从库里取出提示词、交给会话具备的任何生成能力（生成工具 / MCP / 脚本 / 浏览器），不需要你重新粘贴；提示词原文只在你要求看时才展示。

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

输出固定为四段：**可直接投喂的提示词**（一个完整代码块）→ **关键假设** → **可选确认**（≤3 问，不答也没关系）→ **调整方向**；同时自动存入 `prompts/` 提示词库。下次直接说「用上次那个提示词生成视频」即可复用，库文件格式见 SKILL.md 的「提示词库：保存与复用」。

## 文件结构

```
prompt-optimizer/
├── SKILL.md                  # 核心工作流：五要素解析 → 假设处理 → 选结构 → 撰写 → 交付 → 入库/复用
└── references/
    ├── frameworks.md         # 结构框架库：思维链 / few-shot / 输出模板锁定 / 自检清单 + 按任务类型选择
    └── video-image.md        # 图像七要素、视频运镜词汇表、主流生成器写法差异
```

## 可移植性

Skill 本体不依赖任何平台特定能力，全部工作用对话文本完成，产出的提示词在纯文本环境下可用。正文为中文，触发描述中英双语。
