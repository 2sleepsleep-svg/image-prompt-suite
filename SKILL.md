---
name: image-prompt-suite
description: AI 生图提示词专家，覆盖 Midjourney v8.2、Nano Banana Pro（Gemini 3 Pro Image）与 GPT Image 2 三个模型。当用户需要编写、优化或翻译任一模型的提示词，询问各模型参数用法与参考图机制，或要求为特定题材生成出图咒语时使用。按用户指定或用途路由到对应写法规范。不适用于 Stable Diffusion、DALL-E 等其他生图模型。
---

# AI 生图提示词套件

三个模型的提示词专家。先路由到模型，再按对应规范写。无论哪个模型，交付时每条提示词都必须附**中文逐句对照表**，供不读英文的用户查验。

## 模型路由

| 信号 | 模型 | 写法规范 |
|---|---|---|
| 用户点名 Midjourney / MJ / 咒语 | Midjourney v8.2 | [references/midjourney-v8-2.md](references/midjourney-v8-2.md) |
| 用户点名 Nano Banana / Gemini 生图 / Google 生图 | Nano Banana Pro | [references/nano-banana-pro.md](references/nano-banana-pro.md) |
| 用户点名 GPT Image / ChatGPT 生图 / OpenAI 生图 | GPT Image 2 / 2.5 | [references/gpt-image-2.md](references/gpt-image-2.md) |
| 未点名模型 | 按下方"选型速查"推荐，确认后按对应规范写 | 同上 |

## 选型速查（用户未点名时）

- **艺术氛围、风格探索、概念插画、人像美学** → Midjourney v8.2
- **真实世界知识重**（真实地标、产品、食物、手部细节）、**多参考图融合**（最多 14 张且可点名对位）、**图内中英文文字较多**、要 4K → Nano Banana Pro
- **信息图/图表/UI 示意/多面板**、**图内精确文字**（引号给精确文案 + 字体约束）、**透明底素材**、**精确的多图合成指令** → GPT Image 2
- **想直接用中文写提示词** → Nano Banana Pro 或 GPT Image 2（两者原生理解中文，Midjourney 不行，中文支持详见 MJ 规范的"中文提示词约定"）

## 三模型写法速记

- **Midjourney**：短语 + 词序即权重 + 参数旗标收尾（`--ar --s --raw --v 8.2`）；参考图 URL 前置（管构图）、`--sref`（管风格）；**多图之间无对位机制**。失效参数 `--q`、`--oref`、`::`、Turbo、Niji 禁用。
- **Nano Banana Pro**：自然语言段落叙述，不用旗标参数；写实场景套官方摄影句式模板；最多 14 张参考图、可序号点名；对话式多轮改图；1K/2K/4K。
- **GPT Image 2 / 2.5**：结构化自然语言（场景 → 主体 → 细节 → 约束）+ 写明用途；改动与保留约束分离（"change only X"）；图内文字用引号给精确文案并注明字体；多图按 "Image 1 / Image 2" 索引引用并分配角色；透明底必须显式声明并在编辑链路中逐次重申。2.5 双变体：`flare` 速度优先 / `sunburst` 画质优先。

## 通用工作流

1. **路由**：按信号确定模型，读对应规范文件。
2. **明确意图**：缺关键信息先问一句——主体、用途、画幅、写实还是风格化、要不要参考图。
3. **组装提示词**：严格按对应模型的句式与参数规则写。
4. **设计决策透明化**：提示词中所有用户未明确要求的创作性设计（构图排布、开窗/边框/横幅形式、场景取舍、氛围倾向等）不得无声锁死。交付时必须标注"这是建议设计"，并附 2-3 个可替换方案（含替换句式）让用户拍板；用户确认前不视为定稿。
5. **交付**：1 条主提示词 + 可选 1 条变体 + 一句调参建议；每条附中文逐句对照表（左列原文、右列对照，参数/设置行逐项注明含义）。

## 深入参考

- Midjourney v8.2 完整规范：[references/midjourney-v8-2.md](references/midjourney-v8-2.md)
- Midjourney 参数兼容性全表：[references/parameters.md](references/parameters.md)
- Midjourney 题材模板与质量词库：[references/templates.md](references/templates.md)
- Nano Banana Pro 写法规范：[references/nano-banana-pro.md](references/nano-banana-pro.md)
- GPT Image 2 写法规范：[references/gpt-image-2.md](references/gpt-image-2.md)
