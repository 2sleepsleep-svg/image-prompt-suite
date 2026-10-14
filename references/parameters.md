# Midjourney v8.2 参数完整参考

兼容性核对自 Midjourney 官方 Version 文档的 Feature Compatibility 图表（2026-09-01 版本，v8.1 与 v8.2 合并列）。

## 兼容性总表

| 参数 / 功能 | V7 | V8.1 & V8.2 | 说明 |
|---|---|---|---|
| `--ar` 画幅比例 | 可用 | 可用 | 最大 14:1；同时开 `--hd` 时最大 4:1 |
| `--c` Chaos | 可用 | 可用 | |
| `--s` Stylize | 可用 | 可用 | |
| `--w` Weird | 可用 | 可用 | |
| `--tile` | 可用 | 可用 | |
| `--raw` | 可用 | 可用 | |
| `--no` | 可用 | 可用 | 不依赖 `::` 语法 |
| `--seed` | 可用 | 可用 | v8.x 官方口径为"99% 一致"，不保证逐像素一致 |
| `--sref` / `--sw` | 可用 | 可用 | |
| `--iw` 图片权重 | 可用 | 可用 | 默认 1 |
| `--p` 个性化 | 可用 | 可用 | v7 全局档案直接沿用；为 v8.x 单独建的档案不能回用于 v7 |
| `--hd` 高清图 | 不可用 | 可用 | v8.1 引入；1:1 下 2048px，GPU 约 1.3 分钟 |
| Edit Model | 不可用 | 可用 | v8.2 新增，取代 Omni Reference、Character Reference、Retexture |
| 对话模式 Conversational Mode | 可用 | 可用 | |
| `--q` Quality | 可用 | **失效** | 官方 Quality 文档仍只写 v7，属文档滞后而非冲突 |
| Turbo 模式 | 可用 | **失效** | |
| `::` 多提示词加权 | 不可用 | **失效** | |
| `--oref` / `--ow` Omni Reference | 可用 | **失效** | 官方明确指引用 v8.x 的 Edit Model 替代 |
| Niji | 可用 | **失效** | 独立模型线，不能与 v8.1/v8.2 同选 |
| `--sv` 风格参考算法版本 | 可用 | **未验证** | 官方仅发布 v6/v7 文档，v8.x 无公开值，不要猜 |
| Draft Mode | 可用 | **有争议** | 官方 Version 图表标不可用，但 Draft 专文称兼容 7/8.1 并有 "Draft Mode in V8.1 / V8.2" 章节（每提示词 24 张 512x512，仅网页端）。围绕它建工作流前先两份文档都读 |

## 各参数详解

- **`--ar`**：默认 1:1。不接受小数，要写 `--ar 139:100` 而不是 `--ar 1.39:1`。常见选型：人像 2:3 / 4:5，横幅场景 3:2 / 16:9，印刷 1:1。
- **`--c`（chaos，0-100，默认 0）**：数值越高，四张结果彼此以及与字面提示词的差异越大。适合前期探索；目标明确时保持 0。
- **`--s`（stylize，0-1000，默认 100）**：越低越贴字面描述，越高 Midjourney 的艺术诠释越多。写实任务常用 50-200，风格化任务可上探 400+。
- **`--w`（weird，0-3000，默认 0）**：推向非常规、实验性结果。与 `--seed` 不完全兼容。
- **`--raw`**：关闭 Midjourney 默认美学"自动驾驶"。简单提示词产出更接近照片真实；详细提示词会被更字面地执行。
- **`--no`**：排除命名元素。注意复合短语陷阱：`--no modern clothing` 会被拆成 "no modern" 和 "no clothing" 两个排除项并可能误触内容过滤。改为直接写出想要的具体服饰。
- **`--seed`（0-4294967295）**：锁定起始噪声用于 A/B 对比。跨会话不可靠；不要与 Turbo 同用（v8.2 本就不支持 Turbo）。
- **`--repeat` / `--r`**：一次提交跑多次同一提示词。次数按套餐：Basic 2-4、Standard 2-10、Pro/Mega 2-40。仅在 Fast（或 Turbo）模式可用，Relax 下不可用。
- **`--sref` + `--sw`（0-1000，默认 100）**：风格参考，接受图片 URL 或 MJ 内部风格码。`--sw` 控制拉动强度。
- **`--tile`**：产出边缘可无缝衔接的图案砖。不要对平铺图做放大，会破坏接缝。
- **`--p`（Personalization）**：应用在 Personalize 页面选图建立的个性化档案。v7 全局档案可直接用于 v8.1/v8.2；暂无独立的 v8 全局档案。
- **`--hd`**：v8.1 新增的高清出图。1:1 分辨率 2048px，GPU 消耗约 1.3 分钟（SD 为 0.8 分钟）。注意：对 HD 图使用 Pan / Zoom Out / Vary Region 等编辑工具会把结果降回 SD，可用 Upscale 恢复到 HD 分辨率。

## GPU 计费模式

每张图按 GPU 时间计费，分三档：

- **Fast**：消耗月度 Fast 额度（账单日重置）。`--repeat` 必须在此档。
- **Relax**：无限量但排队（官方口径等待可达约 30 分钟），仅 Standard/Pro/Mega 套餐可用。Relax 下不可用：排列提示词、`--repeat`、HD 视频、旧版 Max Upscale。
- **Turbo**：双倍 Fast 时间换约 4 倍速度。**v8.1/v8.2 不支持 Turbo**。

GPU 档位不改变提示词写法，只改变参数提交后是否按预期生效。

## v8.2 主题与 Edit Model

- v8.2 于 2026-07-24 成为默认版本（v8.1 自 6 月 10 日起担任默认仅约 6 周；v8.0 是 3 月 17 日的早期体验版，与 v8.2 同日停用）。
- 重点：美学、画质、个性化。官方描述出图"更有创意、大胆、精致、前卫"。
- **Edit Model** 是 v8.2 的核心新功能，取代 Omni Reference、Character Reference 和 Retexture。用户需要角色一致性或图像局部编辑时，引导使用 Edit Model。
- 个性化理解力增强；个人风格档案跨版本沿用规则见上表。

## 资料来源

- Midjourney 官方 Version 文档：https://docs.midjourney.com/hc/en-us/articles/32199405667853-Version
- Midjourney 官方 Prompt Basics：https://docs.midjourney.com/docs/prompts
- Midjourney 官方 Parameter List：https://docs.midjourney.com/hc/en-us/articles/32859204029709-Parameter-List
- Prompt Architects - Free Midjourney Prompt Generator (V8.2 Parameters Included)：https://prompt-architects.com/blog/539-free-midjourney-prompt-generator-v8-2-parameters-included
- Blake Crosley - Midjourney 8.2 Guide：https://blakecrosley.com/guides/midjourney

以上核对截至 2026-09；Midjourney 官方文档自身存在更新滞后（曾出现同一日期两页对当前版本说法不一）。遇参数行为不符，以官方 Version 文档为最终依据。
