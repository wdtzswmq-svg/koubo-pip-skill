# koubo-pip-skill · 口播精剪（语义画中画）

> 一个给 AI Agent 用的口播视频精剪技能：先把口播剪成干净主线，再**按台词语义**从公开视频平台匹配高度相关的真实视频，做画中画（PiP）穿插，配齐音效落点和交付质检。
>
> An agent skill for editing talking-head videos: a clean base edit, plus **semantic picture-in-picture** — real, highly-relevant clips matched to what the speaker is saying, inserted as PiP B-roll.

## 为什么做这个

口播视频最大的问题是信息密度：嘴上讲了一个案例、一个数据、一个产品，画面却只有一张脸。观众"听到了"但"没看见"，完播率和信任感都吃亏。

这个技能的核心方法只有一句话：**台词讲到哪，画面就给哪的真实证据。** 不是随便铺氛围空镜，而是按词级时间轴切分语义点，逐个点去找强相关的真实视频，2–5 秒一段穿插进主画面。

## 核心方法

| 阶段 | 做什么 |
|---|---|
| 一 · 基础精剪 | 转写对轴、去口误停顿、找真实证据、分镜编排、渲染、抽帧质检 |
| 二 · 语义画中画 | 语义切分 → 按关键词找高相关真实视频 → yt-dlp 下载 → 裁中间去水印 → 四种版式轮换插入 |
| 三 · 音效特效 | 每个 PiP 进出点、特效、转场必有音效落点；BGM 垫底约 -28dB；金句放大、花字强调 |
| 四 · 交付质检 | 成片分辨率 = 源片分辨率；口型/字幕、遮挡、音画同步、水印残留逐项过 |

画中画规格速览：

- 每次 **2–5 秒**，每分钟 **2–5 次**，跟语义走不机械凑数
- 四种版式轮换：角落小窗 / 人物压底素材主画面 / 全屏覆盖 / 人物缩角落（约 1/4 屏）证据占主屏
- 字幕一律**无背景**（白字黑描边），避开底部 12%–18% 平台遮挡区
- 找不到强相关素材时**宁可少插，不硬塞**沾边视频

找素材与去水印的详细 SOP 见 [`references/pip-sourcing.md`](references/pip-sourcing.md)，完整工作流见 [`SKILL.md`](SKILL.md)。

## 快速开始

环境要求：

- 一个支持 `SKILL.md` 技能格式的 AI Agent（如 Claude Code、Codex 等）
- `ffmpeg` / `ffprobe`（裁切、抽帧、合成）
- [`yt-dlp`](https://github.com/yt-dlp/yt-dlp)（下载公开视频素材）

安装：把本仓库克隆到你的 Agent 技能目录，例如：

```bash
git clone https://github.com/wdtzswmq-svg/koubo-pip-skill.git
```

使用：把口播原视频交给 Agent，并触发关键词「剪口播」「加画中画」「口播精剪」，Agent 会按 `SKILL.md` 的四阶段流程执行。

## 设计原则

1. **证据必须真实**——截图、官方页面、录屏；禁用 AI 生成的假界面冒充证据。
2. **强相关优先**——PiP 素材与台词语义强绑定，沾边的不要。
3. **质检看帧不看猜**——每个判断都要有抽帧或波形证据。

## English summary

`koubo-pip-skill` is an agent skill (SKILL.md format) for turning raw talking-head footage into high-density edits. Its core is a semantic picture-in-picture pipeline: the transcript is segmented into information points, each point is matched with a highly relevant real clip from public video platforms (downloaded with yt-dlp, center-cropped to remove platform watermarks), and clips are inserted for 2–5 seconds each, 2–5 times per minute, rotating through four layouts. It also specifies sound-design rules (every PiP entry/exit, effect and transition needs a sound hit; BGM at about −28 dB), subtitle rules (no background, white text with black outline) and a pre-delivery QC checklist. Real evidence only — no AI-fabricated screenshots.

## 致谢

- 基础精剪的质检闭环思路受 SeeCut 类网感精剪工作流启发；
- 素材下载使用开源工具 [yt-dlp](https://github.com/yt-dlp/yt-dlp)。

## License

[MIT](LICENSE)
