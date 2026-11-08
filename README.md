# 课堂录音转写笔记 · Lecture Note Builder

一个把**课堂录音转写稿**变成**能替代重新听课的结构化笔记**的 AI Skill。

不依赖特定模型，纯提示词工程，无脚本、不联网、不读写本地文件。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 它解决什么问题

大多数人和大多数模型拿到转写就按时间顺序复述，产出是流水账：读完还是不知道这节课在讲什么。

这个 skill 的核心判断是——**分水岭不在写，在写之前的两次拆解**：

| 步骤 | 做什么 | 不做会怎样 |
|---|---|---|
| **第一次拆解：定主线** | 一句话回答「这 2–3 小时其实只在讲一件事的哪个版本？」 | 全文没有论点，只剩知识点堆叠 |
| **第二次拆解：重排顺序** | 把时间线打碎，按概念依赖关系重排 | 读者跟着讲师的即兴跳跃走，越读越乱 |

再加一条同等关键：**先列素材清单，再写正文**。这一步同时压制了编造和"没读完就写"。

> 两遍读 → 先清单 → 再定主线 → 重排 → 写 → 自检

---

## 产出长什么样

一篇 2 小时的课，产出大致是：

| 指标 | 数量 |
|---|---|
| 字数 | 1.8 万–2.2 万字 |
| 二级标题 | 12–18 节（判断句/问题式，不是名词） |
| 对比表格 | 15–25 个 |
| 讲师原话引用 | 8–20 处（带时间戳可定位） |
| 编号要点回顾 | 20–35 条 |
| 附加 | 人物与案例索引、笔记说明（数据口径与存疑标注） |

结构：

```text
头部元信息（课程/教师/时间/地点/本场内容）
  ↓
导语：主线 + 与前后场次的关系 + 讲师的方法论立场
  ↓
正文 12–18 节（逻辑顺序，不是时间顺序）
  ↓
要点回顾（编号，20–35 条）
  ↓
附录：人物与案例索引
  ↓
笔记说明（哪些是口述、哪些是反算、哪些听不清）
```

片段示例见 [`examples/sample-excerpt.md`](examples/sample-excerpt.md)。

---

## 安装

### WorkBuddy / 支持 Skill 的客户端

**方式一：技能市场安装**

在技能市场搜索「课堂录音转写笔记」，点 `+` 安装。

**方式二：本地目录**

```bash
git clone https://github.com/tobey-xxx/course-note-from-transcript.git \
  ~/.workbuddy/skills/course-note-from-transcript
```

**方式三：上传 zip**

把仓库打成 zip（`SKILL.md` 必须在包根，不能套一层文件夹），在客户端「技能 → 添加技能 → 上传」导入。

### 其他 AI 工具（Claude Code / Cursor / 任意对话框）

这个 skill 本质是提示词，不绑定平台。两种用法：

- **支持 skills 目录的工具**：把仓库放进对应的 skills 目录即可
- **普通对话框**：直接复制 [`references/quickstart-prompt.md`](references/quickstart-prompt.md) 的 40 行精简版提示词，把转写贴在末尾发送

---

## 使用

最简用法：

```text
这份 2 小时的课程录音转写，帮我整理成课程笔记。
```

Skill 会先输出一份**素材清单**（主线候选、骨架、可引用原话、数字、案例、课堂互动、存疑点），你确认后再写正文。

想一次出稿，加一句：

```text
不用确认，直接出终稿。
```

（跳过确认，但内部仍会先完成素材清单作为取料依据。）

---

## 目录结构

```text
course-note-from-transcript/
├── SKILL.md                          # 技能定义与工作流（入口）
├── references/
│   ├── workflow.md                   # 四步流程详解 + 素材清单模板
│   ├── structure.md                  # 结构模板 + 导语/分节/附录写法
│   ├── writing-techniques.md         # 八条写作手法 + 正反例
│   ├── fact-discipline.md            # 防编造五约束 + ASR 校正边界
│   ├── checklist.md                  # 质检清单 + 验收指标 + 失败模式
│   └── quickstart-prompt.md          # 40 行精简版，可直接粘贴
├── examples/
│   └── sample-excerpt.md             # 成品片段示例
├── README.md
├── CHANGELOG.md
└── LICENSE
```

---

## 设计取舍

**为什么坚持两遍？**
一次 prompt 要终稿，模型会为了填满结构而编造。先要清单，编造空间被压缩，而且清单本身可核对——你能一眼看出它哪里读错了。

**为什么强调"判断标准"？**
笔记的用途是**拿去用**，不是**知道有这回事**。"这个准则分四类"是知识，"遇到这种情况按这个标准判"才是能力。

**为什么数字要标口径、存疑要标注？**
课堂笔记常被二次引用（写报告、做汇报）。一个看起来合理但错的数字，比一个明说"听不清"的空白危险得多。

**为什么保留讲师的"土办法"和情绪？**
「货烧了要不要赔」这种判据，胜过三段准则原文。改写它反而丢掉了它最好用的部分。

**为什么不用 LaTeX？**
大多数笔记平台没有装数学渲染插件，`$$...$$` 和 `\frac` 会原样显示成一堆反斜杠。公式一律纯文本或代码块。

---

## 适用边界

| 适用 | 不适用 |
|---|---|
| 有明确知识结构的讲授型课程 | 纯自由讨论的圆桌（无主线可提炼） |
| 讲师有个人判断和案例的课 | 照本宣科的念 PPT（转写无增量信息） |
| 转写质量可辨识 80% 以上 | 转写质量极差（大量 MISSING） |
| 需要长期查阅、二次引用的场景 | 会议纪要（目标是行动项，用另一套模板） |

---

## 贡献

欢迎提 Issue 说你在什么课上失效了，或 PR 补充新的失败模式与修法。

改 `references/` 时请保持一个原则：**每条规则都要能落到"怎么做"，不能只是"要做好"**。

---

## License

MIT © 2026 tobey-xxx

---

**English TL;DR**

A model-agnostic prompt skill that turns lecture transcripts into structured, review-ready course notes. The method: two-pass reading (material inventory first, prose second), a single thesis statement, logical re-ordering instead of chronological narration, comparison tables over prose, verbatim instructor quotes, and explicit labeling of uncertain or self-contradictory data. Ships with full prompt, workflow, structure template, writing techniques with do/don't examples, anti-fabrication rules, and a QA checklist.
