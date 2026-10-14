# GPT Image 2 写法规范

依据 OpenAI 官方 Cookbook《GPT Image Generation Models Prompting Guide》（gpt-image-2 章节）。来源链接见文末。

## 模型定位

- **`gpt-image-2`**：OpenAI 当前最强图像模型——最高质量生成与编辑，推荐用于：文字密集图、写实、合成（compositing）、身份敏感编辑、首过质量优先的生产工作流。
- `quality` 三档：**low**（低延迟高吞吐，质量已够很多场景）/ **medium** / **high**（小字、密集信息面板、特写肖像、身份敏感编辑）。
- `input_fidelity` 不适用于 gpt-image-2（输出默认已是高保真）。
- **透明底 PNG/WebP（preview）**：`background="transparent"` + PNG/WebP 输出格式。
- 尺寸灵活：满足约束可传任意 `size`；超过 2560x1440（2K）按实验性对待。常用 `1024x1024` / `1024x1536` / `1536x1024` / `auto`；4K `3840x2160` 为实验上限（受最大边约束时取 `3824x2144`）。

## 提示词写法（官方 Prompting Fundamentals）

1. **一致顺序**：背景/场景 → 主体 → 关键细节 → 约束；并**写明用途**（ad、UI mock、infographic）——用途声明会设定"模式"与精细度。
2. **复杂需求用带标签的短段或换行**，不要写成一大段。
3. **人物**：描述比例、取景、视线与交互——"full body visible, feet included"、"child-sized relative to the table"、"looking down at the open book, not at the camera"、"hands naturally gripping the handlebars"。
4. **图内文字**：字面文字放**引号**或全大写，注明字体风格、字号、颜色、位置；品牌名等易错词**逐字母拼写**；小字、密集信息、多字体版式用 medium/high quality。
5. **多图输入——索引式对位**：按"序号 + 描述"引用每张输入图（"Image 1: product photo… Image 2: style reference…"），并**写明交互关系**（"apply Image 2's style to Image 1"）；合成时显式说明元素去向（"put the bird from Image 1 on the elephant in Image 2"）。这是 GPT Image 2 与 Midjourney 的本质区别：**多图可以精确对位**。
6. **透明底**：提示词必须显式要求 isolated subject on a fully transparent background（无场景、无纯色背板、无棋盘格、无阴影）；编辑链路中每一步都要重申保留透明底，防止中途引入新背景。

## 与 Midjourney 的差异速记

| 维度 | GPT Image 2 | Midjourney v8.2 |
|---|---|---|
| 参数 | 无旗标，比例/质量由 API 参数或对话说明 | `--ar --s --raw` 等旗标收尾 |
| 多参考图 | 索引式精确对位（Image 1 / Image 2） | 无对位，整体融合 |
| 改图 | 给图 + 指令，多轮对话 | 重 roll 或 Edit Model |
| 图内文字 | 引号精确文案 + 逐字母拼写难词，可靠性高 | 引号内英文短句尚可，中文乱码率高 |
| 中文提示词 | 原生理解，可直接中文描述 | 不支持，需英文 + 中文对照 |

## 交付约定

同主 SKILL.md：每条提示词附中文逐句对照表；缺关键信息先问一句。

## 资料来源

- OpenAI Cookbook — GPT Image Generation Models Prompting Guide（gpt-image-2）：https://cookbook.openai.com/examples/multimodal/image-gen-models-prompting-guide
- 源文件：https://raw.githubusercontent.com/openai/openai-cookbook/main/examples/multimodal/image-gen-models-prompting-guide.ipynb
