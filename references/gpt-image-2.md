# GPT Image 2 / 2.5 写法规范

主要依据 OpenAI 官方 API 文档《Image prompting》指南；早期 gpt-image-2 细节参照 OpenAI Cookbook 同主题指南。来源链接见文末。

## 模型定位

- **`gpt-image-2`**：上一代旗舰——最高质量生成与编辑，适用于文字密集图、写实、合成（compositing）、身份敏感编辑、首过质量优先的生产工作流。
- **GPT Image 2.5（当前主推）**，两个变体：
  - **`gpt-image-2.5-flare`**：小模型，速度优先，画质与 GPT Image 2 相当——延迟敏感/高吞吐工作流首选
  - **`gpt-image-2.5-sunburst`**：基础模型，画质优先，高于 GPT Image 2——质量要求苛刻的场景首选
  - 两者均支持生成、编辑、透明底；先选模型再调 quality
- **迭代编辑原则（官方）**：把上一次输出作为下一次编辑输入，一次只改一处；重复编辑可能改变本要保留的细节——重申保留约束并逐次检查；要求像素级不变的区域用合成而非提示词。

## 模型参数（与提示词分开设置）

| 参数 | 设置 |
|---|---|
| `model` | `gpt-image-2.5-flare` 或 `gpt-image-2.5-sunburst`（GPT Image 2 亦可用） |
| `quality` | `auto`（默认）、`low`、`medium`、`high`、`xhigh`、`max` |
| `size` | `auto` 或自定义分辨率。常用：`1024x1024`、`1536x1024`（横）、`1024x1536`（竖）、`2048x2048`、`2048x1152`（2K 横）、`3840x2160`（4K 横）、`2160x3840`（4K 竖） |
| `background` | `auto`、`opaque`、`transparent` |

自定义分辨率约束：每边 ≤3840px；两边均为 16 的倍数；长短边比 ≤3:1；总像素 655,360–8,294,400；超过 `2560x1440`（2K）按实验性对待。

- **透明底**：显式 `background="transparent"` + PNG/WebP 输出；检查解码后 alpha 通道（发丝、玻璃、阴影、边缘）；`output_compression` 只用于 JPEG/WebP，不用于 PNG。
- 对比测试时保持 quality/prompt/参考图/尺寸不变；同一 quality 标签在不同模型间不代表同等画质或耗时。

## 提示词写法（官方 Fundamentals，8 条）

1. **定义结果**：点名主体与用途（产品照、广告、图表），指定构图、比例、位置约束；复杂需求按 scene / subject / details / constraints 分节组织。
2. **选择可维护的格式**：短句、描述段落、类 JSON 结构、指令、标签皆可，选最好读改的，不要依赖特殊语法。
3. **描述可见细节**：点名材质、光线、颜色、视觉媒介；写实就明说 "photorealistic" / "real photograph"；相机规格是**外观提示**而非物理模拟保证；宽幅/电影感/暗光/雨夜/霓虹场景要写尺度、氛围与色彩，别只堆情绪词。
4. **人物与动作**：描述取景、相对比例、视线与物体交互——"full body visible, feet included"、"looking down at the open book"、"hands naturally gripping the handlebars"。
5. **精确文字**：要出现的文字放**引号**，说明位置与字体；生僻词/品牌名**逐字母拼写**；要求"不要多余文字"并在出图后核对；小字/密集信息/多字体用 medium 或 high。
6. **改动与约束分离**：编辑时说 "change only X"，并列出必须保留的细节（身份、几何、版式、光线、标签）；声明排除项（多余文字、logo、水印）；局部精修时连饱和度、对比度、箭头、机位、周边物体都要点名保持不变。
7. **给参考图分配角色**：按"序号 + 用途"引用（主体 / 风格 / 服装 / 背景），并写明如何组合、哪个元素从哪张图移到哪张图。
8. **刻意迭代**：上一轮输出作为下一轮输入，一次一改，重申保留项；结果漂移就重申关键约束；加指令前先对比结果。

## 与 Midjourney 的差异速记

| 维度 | GPT Image 2 / 2.5 | Midjourney v8.2 |
|---|---|---|
| 参数 | 无旗标，模型/质量/尺寸由 API 参数或对话说明 | `--ar --s --raw` 等旗标收尾 |
| 多参考图 | 序号 + 角色分配，精确对位 | 无对位，整体融合 |
| 改图 | 上轮输出作下轮输入，一次一改，多轮对话 | 重 roll 或 Edit Model |
| 图内文字 | 引号精确文案 + 逐字母拼写难词，可靠性高 | 引号内英文短句尚可，中文乱码率高 |
| 中文提示词 | 原生理解，可直接中文描述 | 不支持，需英文 + 中文对照 |

## 交付约定

同主 SKILL.md：每条提示词附中文逐句对照表；缺关键信息先问一句。

## 资料来源

- OpenAI 官方 API 文档 — Image prompting（GPT Image 2.5）：https://developers.openai.com/api/docs/guides/image-prompting
- OpenAI 官方 API 文档 — Image generation（API 设置）：https://developers.openai.com/api/docs/guides/image-generation
- OpenAI Cookbook — GPT Image Generation Models Prompting Guide（gpt-image-2）：https://cookbook.openai.com/examples/multimodal/image-gen-models-prompting-guide
