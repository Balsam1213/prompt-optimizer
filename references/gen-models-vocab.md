# 模型适配与词汇银行（gen-models-vocab）

> 整合自 [q253595422/prompt-constraints](https://github.com/q253595422/prompt-constraints)（MIT）。模型能力边界以官方文档为准，本文是「提示词该长什么样」的经验归纳。

## 一、通用适配原则

1. **形态跟着模型走**：自然语言派（Gemini / GPT Image / Veo / Seedance / Sora）→ 完整句子讲清关系；标签派（Midjourney / SD / FLUX）→ 短词组 + 逗号分隔 + 权重语法 `(word:1.2)`。
2. **负面词位置不一样**：有独立 negative prompt 字段的（SD / FLUX / 部分国产模型）→ 分开写；只吃单一 prompt 的（Midjourney / GPT Image / Gemini）→ 用「包含排除结果的场景描述」写进正文。
3. **参数写法不一样**：Midjourney 用 `--ar 16:9 --style raw`；其余模型参数在 UI/API 里设，不进提示词。
4. **语言**：中文友好的（即梦 Seedream / 可灵 / Seedance / MiniMax）可直接中文；其余优先英文。

跨模型迁移四步：**拆解**（还原成维度清单，别逐字翻译）→ **重排**（按目标模型形态重组）→ **补约束**（媒介边界、参考资产映射、官方字段）→ **去噪音**（删源模型专属语法与参数）。不要把 `--ar 16:9` 贴给 Gemini，也不要把 600 字长句贴给 SD。

## 二、生图模型速查

| 模型 | 形态 | 偏好 | 避坑 |
|---|---|---|---|
| **Nano Banana Pro**（Gemini 图像） | 自然语言长句，中文可用 | 强指令跟随、强文字渲染、强编辑 | 描述过碎会稀释；编辑要写「只改 X 其余不变」 |
| **即梦 Seedream** | 中文自然语言 | 中文语境、海报排版、国风强 | 过长英文技术词收益低 |
| **GPT Image** | 自然语言指令式 | 指令跟随强、文字排版准、数量控制精确 | 别用 MJ 参数语法；数量位置精确写 |
| **Midjourney** | 短词组 + 参数 | 美学风格强、`--sref` `--cref` 体系 | 不吃长句、`--no` 有限、英文优先 |
| **FLUX / SD** | 标签式 + 权重 | 独立负面字段、LoRA/ControlNet 生态 | 要正确的权重与负面语法 |
| **Ideogram** | 自然语言 + 显式文字 | 文字/Logo/海报最强 | 用引号给出要渲染的文字 |

## 三、视频模型速查

| 模型 | 形态 | 偏好 | 避坑 |
|---|---|---|---|
| **Seedance** | 导演式 + 显式参考 + 时间节拍，可用 `Setting:/Action:/Camera:/Style:/Audio:` 分段 | 多模态参考、多镜头、音频 | 短片段别堆动作；参考资产逐个声明角色 |
| **可灵 Kling** | 中文友好，动作直白 | 运动控制、首尾帧、物理可信 | 动作避免抽象；明确镜头是否固定 |
| **Veo 3** | 五段式：`[电影摄影]+[主体]+[动作]+[上下文]+[风格氛围]` | 原生音频、首尾帧转场 | 负面写成场景描述；不写模型名/时长/比例 |
| **Sora** | 自然语言，可长 | 长镜头、复杂物理、叙事连续 | 别堆关键词；写清镜头逻辑 |
| **MiniMax Hailuo** | 保留官方字段名与行序 | 对齐指令、镜头时间戳 | schema 字段不能擅自简化 |
| **Wan** | 自然语言 + 运动描述 | 动画与角色驱动 | 动作具体到关节级更稳 |

## 四、词汇银行（词穷时查，精准 > 华丽 > 数量）

一个「光比 4:1 的硬质侧光」胜过五个「电影感 高级 氛围 大片 质感」。按需取 3—5 个精准词。

**艺术风格**：电影感写实 Cinematic Realism ｜ 商业广告质感 Commercial Look ｜ 复古胶片 Vintage Film ｜ 赛博朋克 Cyberpunk ｜ 水墨国风 Chinese Ink ｜ 暗黑哥特 Dark Gothic ｜ 超现实主义 Surrealism ｜ 极简主义 Minimalism ｜ 厚涂油画 Impasto Oil ｜ 二次元赛璐璐 Anime Cel ｜ 3D 写实渲染 Photorealistic 3D ｜ 像素艺术 Pixel Art ｜ 黏土定格 Claymation

**色彩**：霓虹冷调 Neon Cool ｜ 金红暖调 Gold-Red Warm ｜ 莫兰迪灰调 Morandi Muted ｜ 马卡龙粉彩 Pastel ｜ 黑白高对比 High-Contrast B&W ｜ 赛博紫蓝 Cyber Purple-Blue ｜ 深海蓝绿 Deep Ocean Teal ｜ 复古棕褐 Vintage Sepia ｜ 双色调 Duotone。写色彩用三段式：主色 → 辅助色 → 点缀色。

**光照**：黄金时刻 Golden Hour ｜ 蓝调时刻 Blue Hour ｜ 三点布光 Three-Point ｜ 硬质侧光 Hard Side Light ｜ 逆光剪影 Backlight Silhouette ｜ 轮廓光 Rim Light ｜ 体积光束 Volumetric Beams ｜ 丁达尔效应 Tyndall Rays ｜ 霓虹 Neon ｜ 窗光 Window Light ｜ 伦勃朗光 Rembrandt ｜ 明暗对比法 Chiaroscuro

**镜头构图**：特写 Close-up ｜ 中景 Medium Shot ｜ 远景 Wide Shot ｜ 极低机位 Extreme Low Angle ｜ 鸟瞰 Bird's-eye ｜ 荷兰角 Dutch Angle ｜ 三分法 Rule of Thirds ｜ 框架式 Framing ｜ 引导线 Leading Lines ｜ 浅景深 Shallow DoF ｜ 微距 Macro ｜ 前景遮挡 Foreground Occlusion ｜ 负空间留白 Negative Space

**材质**：次表面散射 Subsurface Scattering ｜ 磨砂玻璃 Frosted Glass ｜ 拉丝金属 Brushed Metal ｜ 做旧皮革 Weathered Leather ｜ 丝绒 Velvet ｜ 陶瓷釉面 Glazed Ceramic ｜ 混凝土 Raw Concrete ｜ 湿润表面 Wet Surface ｜ 皮肤毛孔纹理 Skin Pores ｜ 胶片颗粒 Film Grain

**情绪（必须配具体视觉来源）**：宁静 Serene ｜ 压抑 Oppressive ｜ 史诗感 Epic ｜ 孤寂 Solitary（=广角空旷 + 冷调低饱和 + 单一微小主体）｜ 张力 Tense ｜ 神圣 Sacred ｜ 怀旧 Nostalgic ｜ 梦幻 Dreamlike

**场景**：雨夜霓虹街巷 Rainy Neon Alley ｜ 古刹禅院 Temple Courtyard ｜ 星舰舰桥 Starship Bridge ｜ 荒原废墟 Post-Apocalyptic Ruins ｜ 竹林小径 Bamboo Path ｜ 悬浮群岛 Floating Islands ｜ 极简白色影棚 Minimal White Studio

**后期**：电影调色 Cinematic Grading ｜ 青橙调 Teal and Orange ｜ 柔焦 Soft Focus ｜ 辉光 Bloom ｜ 色差 Chromatic Aberration ｜ 镜头眩光 Lens Flare ｜ 运动模糊 Motion Blur

> `8K`、`超高细节` 只能作为期望生成质量，不能声称是原图真实参数。
