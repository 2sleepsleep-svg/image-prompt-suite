# Midjourney v8.2 写法规范

帮用户把出图需求转写成符合 Midjourney v8.2 规则的提示词，并规避 v8.2 已失效的旧参数。写提示词时像向专业摄影师描述画面，而不是堆砌关键词。

## v8.2 关键变化

v8.2 于 2026-07-24 成为默认版本，重点是美学、画质与个性化：

- 新增 **Edit Model**，取代了旧版的 Omni Reference、Character Reference 和 Retexture。用户要角色一致性或局部修改时，引导其使用 Edit Model，而不是旧参数。
- 以下旧参数在 v8.2 **全部失效**，任何提示词里都不要出现：
  - `--q`（Quality）
  - `--oref` / `--ow`（Omni Reference）
  - `::` 多提示词加权语法（`--no` 不受影响，仍可用）
  - Turbo 模式
  - Niji（独立模型线，不能与 v8.1/v8.2 同用）
  - `--sv`（仅 v6/v7 有文档，v8.x 未验证，不要猜值）

## 工作流

1. **明确意图**：缺关键信息时先问一句——主体是什么、用途/题材、画幅比例、偏写实还是偏风格化。用户给了完整描述就直接写。
2. **按模板组装**：用下面的六层结构填写，删除方括号。
3. **追加参数**：参数串放在最末尾，一次性写完。
4. **交付时给出**：1 条主提示词 + 可选的 1 条变体（如更高 `--s` 或不同 `--ar`），附一句调参建议；每条提示词后必须附**中文逐句对照表**（左列英文原文、右列中文对照，参数行逐项注明含义），供不读英文的用户查验内容。

## 提示词模板

```
[主体 — 谁/什么，具体到两个不同的人会想象出同一个画面]

Setting: [场景 — 地点、时间、天气]

Style: [媒介 — 照片/油画/3D渲染/水墨插画等]
[光线 — 黄金时刻/柔光箱/正午硬光/霓虹等]

Composition: [景别 — 特写/广角/俯拍] [视角]

Mood: [一到两个情绪形容词]

--ar [比例] --s [0-1000] --c [0-100] --raw --v 8.2
```

填写示例：

```
A weathered lighthouse keeper in his sixties, salt-stained wool sweater,
binoculars around his neck, standing at the top rail of a lighthouse.

Setting: A rocky North Atlantic coastline at dusk, storm clouds breaking,
one shaft of sunlight hitting the water.

Style: Cinematic photograph, medium format film grain.
Backlit by the breaking sunlight, cool blue ambient fill.

Composition: Medium shot, slight low angle looking up at him.

Mood: Weathered, solitary, resolute.

--ar 3:2 --s 150 --c 10 --raw --v 8.2
```

## 参数速查（v8.2 可用）

| 参数 | 范围/默认 | 作用 |
|---|---|---|
| `--ar` | 默认 1:1 | 画幅比例，整数比（写 139:100，不写 1.39:1），最大 14:1（开 `--hd` 时 4:1） |
| `--s` | 0-1000，默认 100 | 风格化强度，越低越贴字面，越高越有 MJ 自己的艺术发挥 |
| `--c` | 0-100，默认 0 | 四张图之间的差异度，探索期调高，目标明确时保持 0 |
| `--w` | 0-3000，默认 0 | 推向非常规实验性结果，与 `--seed` 不完全兼容 |
| `--raw` | 开关 | 关闭 MJ 默认美学加成，简单提示词更接近照片真实 |
| `--no` | — | 排除元素；避免复合短语（`--no modern clothing` 会被拆成两个排除项） |
| `--seed` | 0-4294967295 | 锁定起始噪声用于对比测试，v8.x 结果"99% 一致"而非保证一致 |
| `--repeat` / `--r` | 按套餐 | 一次提交跑多次；仅 Fast 模式可用（Basic 2-4、Standard 2-10、Pro/Mega 2-40 次） |
| `--sref` + `--sw` | URL 或风格码；sw 0-1000 默认 100 | 风格参考 |
| `--iw` | 默认 1 | 图片提示词权重 |
| `--p` | 开关 | 个性化；v7 的全局档案直接沿用到 v8.1/v8.2 |
| `--tile` | 开关 | 无缝平铺图案；平铺图不要放大，会破坏接缝 |
| `--hd` | 开关 | 高清图（v8.1+ 新增），1:1 时 2048px，GPU 消耗约 1.3 分钟 |

GPU 模式注意：Relax 模式下 `--repeat`、排列提示词、HD 视频均不可用；Turbo 在 v8.1/v8.2 不受支持。

## 硬性规则

- 参数必须放在提示词**最后**，每个 `--` 前留一个空格，参数内部不加逗号或句号。参数后面再写描述文字会静默失效。
- 拒绝关键词堆砌：不要 "beautiful, stunning, 8k, masterpiece" 这类词，v8.2 会因此退化。
- 篇幅 50-150 词最佳，超过 150 词通常收益递减。
- 词序即权重：越靠前的词影响越大，主体永远放最前。
- 矛盾描述会互相抵消（如 "dark, bright, moody, cheerful"）。

## 中文提示词约定

- v8.2 对纯中文提示词支持不可靠：不报错但语义打折，抽象词、成语基本没有视觉锚点。默认一律交付英文提示词 + 中文逐句对照，不要因为"用户是中文使用者"就直接写中文提示词。
- 用户坚持用中文时，给中英混写版（主体、场景保留中文，风格、媒介、摄影术语锚定英文），并说明稳定性不如纯英文。
- 图内中文文字渲染不可靠；需要图内出现中文（如品牌名、标题）时，建议出图后用修图工具加字。

## 深入参考

- 参数兼容性全表与失效参数详解：[references/parameters.md](references/parameters.md)
- 六层结构详解、题材模板与质量词库：[references/templates.md](references/templates.md)

## 资料来源

参数与版本信息核对自 Midjourney 官方文档（2026-09 版）及第三方 v8.2 指南，来源见 references/parameters.md 末尾。若用户报告参数行为不符，建议其以官方 Version 文档为准。
