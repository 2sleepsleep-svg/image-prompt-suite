# Nano Banana Pro（Gemini 3 Pro Image）写法规范

依据 Google 官方文档：Gemini API Image generation（ai.google.dev）、Google Cloud Blog《Ultimate prompting guide for Nano Banana》、Gemini Apps Help。来源链接见文末。

## 模型定位

- **Nano Banana Pro = `gemini-3-pro-image`**：Google 旗舰生图模型，定位最复杂的视觉任务——最强世界知识、高级定位（grounding）、品牌一致性、精确创意控制。
- 家族速览：Nano Banana 2.1（高效主力，推荐新项目）、2 Lite（最快最便宜，不优化多参考）、2（上一代主力）、**Pro（旗舰）**、2.5 Flash Image（legacy）。
- 根本差异于 Midjourney：**对话式生成与编辑**——文生图、图+文改图、多轮逐步迭代，改图靠继续对话而不是重 roll。
- 所有生成图含 SynthID 水印。

## 核心能力（官方文档）

| 能力 | 说明 |
|---|---|
| 参考图 | 最多 **14 张**，可保持人物/主体/产品一致性 |
| 分辨率 | 1K / 2K / **4K**（Pro 支持 4K） |
| Grounding | 可开 Google Search，把实时信息（如当天天气、真实外观）画进图 |
| 文字渲染 | 强项，海报/标题/招牌类文字可靠性高 |
| Thinking | 复杂排版任务有思考过程，可用 thinking level 调节 |

## 提示词写法（官方依据）

1. **自然语言段落叙述**，具体描述用于落实用户需要的控制；未指定部分按主文件保留创作空间。不堆关键词、没有任何 `--` 旗标参数。
2. **写实场景可参考摄影句式模板**：按需取用，不必补齐机位、镜头或光线字段。

```
A photorealistic [type of shot] of a [subject description] in a [setting
description]. [Description of the light]. Shot from a [camera angle] with
a [lens type].
```

3. **摄影词汇按需使用**：用户要求特定摄影效果时，可描述机身效果、镜头、光圈或光线；不默认指定相机参数来限制探索。
4. **图内文字**：把要出现的文字放进引号，注明字体风格、位置；版式复杂时描述层级关系。
5. **编辑**：一次一个明确指令——加什么、删什么、改什么；多轮逐步逼近，每轮只改一处。
6. **多参考图**：用序号点名（如"给第 2 张图里的人物穿上第 1 张图的服装"），明确图与图之间的关系。
7. 开头可用 draw / generate / create 类动词（Gemini Apps 官方建议），并描述想要的风格（photorealistic / charcoal drawing / watercolour 等）。
8. **语义蒙版局部编辑**（官方 Cloud 指南）：用文字划定蒙版——"只替换画面左侧的天空，其余保持不变"，实现精准局部修改而其余区域不动。
9. **Grounding 工作流**：需要真实信息时先让模型检索（"use search to find…"），再指定如何把检索结果可视化。
10. **材质按要求具体化**：用户已指定面料、颜色或纹饰时准确写出；只要求 "suit jacket" 时，不擅自扩写成 "navy blue tweed"。具体材质是控制选项，不是每条提示词的必填项。

## 参数（API 与对话端）

- **aspect ratio**：API 用 `image_config.aspectRatio`（1:1、2:3、3:2、3:4、4:3、4:5、5:4、9:16、16:9、21:9）；对话端（Gemini 应用/AI Studio）可在提示词里直接说明比例或选预设。
- **resolution**：1K/2K/4K，按用途选择；正式印刷用 4K。
- **grounding with Google Search**：需要实时事实（天气、新闻、真实人物外观）时开启。
- 对话式改图无参数，指令即提示词。

## 中文支持

- 原生理解中文提示词，可直接用中文描述，无需先译成英文。
- 图内中文文字渲染可靠性远高于 Midjourney，但复杂版式仍建议：精确文案放引号 + 注明字体 + 出图后检查。

## 交付约定

同主 SKILL.md：每条提示词附中文逐句对照表；缺关键信息先问一句。

## 资料来源

- Gemini API Image generation 官方文档：https://ai.google.dev/gemini-api/docs/image-generation
- Google Cloud Blog — Ultimate prompting guide for Nano Banana：https://cloud.google.com/blog/products/ai-machine-learning/ultimate-prompting-guide-for-nano-banana/
- Gemini Apps Help — Generate and edit images：https://support.google.com/gemini/answer/14286560
