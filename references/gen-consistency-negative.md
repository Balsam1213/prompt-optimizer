# 一致性与排除约束（gen-consistency-negative）

> 整合自 [q253595422/prompt-constraints](https://github.com/q253595422/prompt-constraints)（MIT）。前置阅读：`gen-core.md`（六层约束，本文件是 L4 与 L5 的展开）。

# 一致性（L4：锁变量）

跨图、跨镜、跨批次保持一致，靠的不是模型记性，是**显式声明的不变量**。

## 三层锚点

| 层 | 要锁的东西 | 手段 |
|---|---|---|
| **角色** | 脸型五官、发型发色、体型、肤色、服装、配饰、道具 | 角色参考图（character sheet）+ 不变量声明 |
| **场景** | 空间结构、家具位置、光线方向、色调、天气时间 | 场景参考图 + 空间锚点描述 |
| **风格** | 媒介、渲染方式、色调、颗粒、笔触、后期 | 风格锚点词 + 参考图 |

规则：**能锁的尽量锁，只把真正要变的写成变量。变量越少，一致性越高。**

## 不变量声明模板（通用）

```
以下保持不变：主体脸部与五官、发型发色、体型、服装与配饰、构图/裁切、
光线方向与软硬、色调与饱和度、背景结构与元素、整体风格与媒介。

只改变：<明确列出的变量>。

不要添加或移除：<主目标/关键元素>。
```

## 角色工作表（character sheet）工作流

同一角色出现在多图/多镜头时，先产出一张可复用的参考表：

```
生成角色工作表 → 用工作表作参考生成首个场景静帧 → 从静帧生成图生视频镜头
```

写作顺序：声明输出是角色工作表 → 用具体视觉语言定义身份（年龄段、体型、脸型五官、瞳色、显著特征）→ 锁定服装与轮廓（领型、长度、材质、颜色）→ 指定分格：全身正面 ｜ 全身 3/4 ｜ 侧脸 ｜ 背面（可选表情行、手部姿态、道具特写）→ 跨分格一致性约束 → 纯色/中性灰背景，均匀布光。

**一表一角色**；用户已有图时，把那张图当身份锚点，不要重新发明角色。

**写实身份表模式**（真人、演员连续性）：保留面部不对称，不要平均成通用美颜模；保留年龄线索、皮肤纹理、小瑕疵；目标是「同一个人被反复拍摄」，不是「重新设计的数字资产」。

## 多主体计数约束

有 N 个主要人物/目标时：必写两句——`恰好 [N] 个主要人物/目标` + `不要增加或减少主目标`。超过 6 个目标用分组、手势、眼神代替逐人台词；背景群众只在被要求时存在。

## 漂移诊断

| 漂移症状 | 原因 | 修法 |
|---|---|---|
| 第二张图脸不一样 | 只有文字描述，无参考图 | 补 character sheet + 引用 |
| 服装细节每张都变 | 服装描述太笼统 | 锁定轮廓与关键单品 |
| 光线方向反了 | 光线未列入不变量 | 写死主光方向 + 光比 |
| 角色像「美颜模板」 | 缺不对称与瑕疵描述 | 补面部不对称、皮肤纹理 |
| 主目标数量变了 | 无计数约束 | 补「恰好 N 个 + 不得增减」 |
| 场景结构飘移 | 无空间锚点 | 写死前中后景关键结构与相对位置 |

---

# 排除约束（L5：划红线）

负面提示词不是许愿池，**只解决「模型确实会犯、且我明确知道要排除」的错误**。

## 第一原则：具象化

抽象否定几乎无效——把排除项**写进场景描述**，用「包含排除结果的完整画面」表达：

| 差 | 好 |
|---|---|
| `no man-made structures` | `荒芜的景观，没有建筑和道路` |
| `不要现代元素` | `古代街道，两侧是木构店铺和石板路，画面中没有任何现代建筑、电线或车辆` |
| `不要文字` | `纯视觉画面，画面内没有任何可读文字、标识或水印` |

## 分层组织（按当前风险定制，不套固定套餐）

| 类别 | 典型排除项 |
|---|---|
| 错误媒介 | 与目标媒介冲突的特征（写实图出现 CG 感） |
| 结构缺陷 | 畸形手、多指、脸部崩坏、穿模、比例失调 |
| 内容错误 | 多余人物/肢体、缺少主体、元素悬浮 |
| 光影错误 | 过曝、欠曝、光源方向矛盾、阴影方向错误 |
| 画质缺陷 | 低分辨率、噪点、伪影、过度磨皮、塑料感 |
| 文字标识 | 可读 Logo、车牌、水印、乱码错字 |
| 视频专属 | 跳切、瞬移、倒放、身份漂移、主目标数量变化、场景漂移、POV 错误、结构闪烁 |

## 通用词池（按需取用，8—15 条，英文逗号分隔）

```
low resolution, blurry, jpeg artifacts, oversharpened, over-smoothed,
deformed hands, extra fingers, fused fingers, asymmetric face,
extra limbs, floating objects, bad anatomy, unnatural pose,
overexposed, underexposed, contradictory shadows,
plastic skin, wrong reflections, material confusion,
readable text, watermark, signature, logo, garbled characters,
cluttered background, bad composition, awkward crop
```

**按媒介追加**：摄影加 `3d render, cgi, cartoon, illustration`；写实 3D 加 `real photograph, anime, cel shading`；二次元加 `photorealistic, real photo`；产品渲染加 `lifestyle snapshot, cluttered scene`。

**按主体追加**：人物加 `malformed ears, bad teeth, uneven eyes`；动物加 `extra paws, wrong fur direction, human-like pose`；文字类加 `misspelled text, distorted letterforms`；建筑加 `impossible geometry, warped perspective`。

**视频专属池**：`jump cuts, teleporting, reversed motion, identity drift, face swap, changing number of main targets, scene drift, wrong POV, excessive camera shake, flickering structures, deformed landmarks, visible annotations, visible route lines, numbers on screen, UI overlays, map view`

## 反模式

| 反模式 | 修正 |
|---|---|
| 抄固定 30 词套餐 | 按当前风险定制 8—15 条，说得出每条为什么在 |
| 负面词和正面描述矛盾 | 逐条对照，删冲突项 |
| 把风格需求写成负面词（「不要卡通」） | 用正面描述定义（「写实摄影」），负面只做补充 |
| 用负面词承担一致性 | 一致性用不变量声明 + 参考图 |
| 用它修主体意图（「不要悲伤」） | 直接写「开心大笑」 |
