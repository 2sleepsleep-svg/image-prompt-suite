# 提示词结构与题材模板

## 六层结构（词序即权重）

越靠前的词影响力越大，按此顺序组装：

```
1. 主体（谁/什么）              ← 最重要
2. 主体细节（外貌、材质、状态）
3. 场景（在哪里、何时、天气）
4. 风格/情绪（媒介与氛围）
5. 技术层（相机、镜头、光线）
6. 参数（--ar --s 等）          ← 微调，放最末
```

示例（对应各层）：

```
elderly fisherman                          ← 1 主体
weathered face, silver beard, kind eyes    ← 2 细节
on a wooden dock at dawn                   ← 3 场景
documentary photography, contemplative     ← 4 风格/情绪
shot on Leica, natural morning light       ← 5 技术
--ar 3:2 --s 100 --v 8.2                   ← 6 参数
```

## 篇幅控制

| 词数 | 效果 | 适用 |
|---|---|---|
| 10-30 | 开放解读 | 抽象、实验 |
| 30-80 | 平衡控制 | 大多数提示词 |
| 80-150 | 精细控制 | 具体场景 |
| 150+ | 收益递减 | 易产生冲突 |

超过 150 词通常说明形容词堆砌过多，先做减法。

## 质量词库（v8.x 对以下模式响应强烈）

**光线（影响最大）**
- golden hour light casting long shadows across weathered stone
- Rembrandt lighting with soft fill from camera left
- bioluminescent glow illuminating the fog

**材质与纹理**
- oxidized copper with verdigris patina
- worn leather showing decades of use
- translucent jade catching the light

**氛围与情绪**
- melancholic twilight atmosphere
- oppressive industrial ambiance
- ethereal dreamlike quality

**技术摄影术语**
- shot on medium format, shallow depth of field
- 85mm lens, f/1.8 aperture
- anamorphic lens flare, 2.39:1 aspect

## 题材模板

以下模板均按六层结构填写，参数串可按需调整。

### 电影感写实

```
[主角与动作], [服装/状态细节]
Setting: [具体地点], [时间与天气]
Style: Cinematic photograph, [胶片/数字质感], [主光线描述]
Composition: [景别], [镜头/机位暗示]
Mood: [两个情绪词]
--ar 21:9 --s 150 --c 5 --raw --v 8.2
```

### 人像摄影

```
[人物身份+年龄+神态], [服饰与配饰细节]
Setting: [背景环境], [光线条件]
Style: [85mm/135mm] portrait photograph, shallow depth of field, [光位：Rembrandt / butterfly / window light]
Composition: [headshot / medium shot], eye-level
Mood: [情绪词]
--ar 4:5 --s 100 --raw --v 8.2
```

### 产品摄影

```
[产品名+材质+工艺细节]
Setting: [背景面/展台], [布光：softbox / gradient backdrop]
Style: Commercial product photography, [表面反光/阴影质感]
Composition: [45-degree hero shot / top-down flat lay], centered
Mood: clean, premium
--ar 1:1 --s 50 --raw --v 8.2
```

写实产品图务必带 `--raw`，`--s` 压低，避免 MJ 自由发挥材质。

### 奇幻 / 科幻

```
[主体与奇幻/科幻设定], [独有视觉记号]
Setting: [世界观场景], [奇观元素]
Style: [概念艺术/插画/电影概念图], [光线：volumetric god rays / neon rim light]
Composition: [史诗广角 / 低角度仰视]
Mood: [两个情绪词]
--ar 16:9 --s 400 --c 15 --v 8.2
```

风格化题材可放开 `--s`（300-600），不一定加 `--raw`。

### 建筑与空间

```
[建筑类型+结构特征], [主要材质]
Setting: [区位与环境], [时刻与光线]
Style: Architectural photography, [直线条/透视控制], [自然光描述]
Composition: [one-point perspective / exterior wide shot], [机位高度]
Mood: [两个情绪词]
--ar 3:2 --s 100 --raw --v 8.2
```

### 抽象 / 实验

```
[核心视觉概念], [主导形态与色彩关系]
Setting: 无具体场景时可直接省略
Style: [媒介：fluid art / long exposure / generative pattern]
Composition: [全幅充满 / 负空间留白]
Mood: [两个情绪词]
--ar 1:1 --s 600 --c 40 --w 1000 --v 8.2
```

实验方向可组合 `--c` 与 `--w` 探索，锁定方向后再归零。

## 常见错误清单

1. **关键词堆砌**："beautiful, stunning, 8k, masterpiece" 会让 v8.2 结果平庸化，删掉。
2. **参数后置失效**：描述文字出现在参数之后（如 `vibrant California --ar 2:3 poppies`）会让参数静默失效，参数必须收尾。
3. **主体不具体**："an old man" 不如 "a weathered lighthouse keeper in his sixties"；两张图都不具体时，先追问再写。
4. **矛盾修饰**："dark, bright, moody, cheerful" 互相抵消。
5. **复合 `--no`**：`--no modern clothing` 被拆成两个排除项；改为写出想要的正面描述。
6. **沿用过时参数**：`--q`、`--oref`、`::`、Turbo 在 v8.2 全部失效，详见 parameters.md 兼容表。
7. **出图平庸时**：先检查每个占位符是否给了真正具体的答案，而不是先调参数——参数只控制画框，不拯救空洞的主体描述。
