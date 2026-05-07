# 📝 Reduce AIGC Rate — 论文AI率降低工具

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green.svg)](https://nodejs.org/)

一个 Claude Code Skill，用于降低学术论文的 AIGC（AI生成内容）检测率。自动提取 Word 文档章节，逐章进行 AI 痕迹消除改写，再回写到原文档。

> **🐍 Python 版** — 基于 `python-docx`，docx 兼容性最好，推荐使用。
>
> **🟢 Node.js 版** — 零依赖，适合没有 Python 的用户（计划中）。

## ✨ 特性

- **自动章节识别** — 基于 Word 标题样式(Heading 1/2/3)自动切分章节
- **逐章改写** — 每章独立送入 AI 改写，避免上下文窗口爆掉
- **11条改写规则** — 调整语序、替换同义词、变换主被动态、拆分长句、弱化专业感……
- **保留格式** — 回写时保留原标题、字体、样式、表格
- **自动备份** — 修改前自动备份为 `原文件名_backup.docx`
- **字数控制** — 改写前后字数变化不超过 ±5%

## 📦 安装

### 方式一：手动安装

```bash
# 克隆到 Claude Code skills 目录
git clone https://github.com/ZYGggg666/reduce-aigc-rate.git ~/.claude/skills/reduce-aigc-rate
```

### 依赖安装

**Python 版（推荐）：**

```bash
pip install python-docx
```

系统需要 Python 3.8+。[Python 官网下载](https://www.python.org/downloads/)

## 🚀 使用

在 Claude Code 中：

```
/reduce-aigc-rate 帮我降低 d:/论文/我的论文.docx 的AI率
```

Skill 会自动完成以下步骤：

```
用户提供 .docx 路径
    ↓
① 提取章节 → 按标题样式切分，生成 chapter_01_xxx.txt ...
    ↓
② 逐章改写 → AI 按11条规则重写每个章节
    ↓
③ 回写Word → 替换原文档对应章节内容
    ↓
④ 完成 → 原文件已备份为 xxx_backup.docx
```

## 🔧 改写规则

改写围绕三个维度，目标是让文本读起来像一个**写起来很吃力的普通学生**，而非 AI：

### 结构变化
- 拆分 >30字的长句，多用逗号、分号衔接
- 打乱语序：状语前后互换、因果倒置、主谓换位
- 把字句/被字句随机互换
- 补充无主语句的主语（本文/该研究/笔者）

### 词汇替换
- 高频AI词降级：显著→比较明显、进行→做、实现→做到
- 专业术语通俗化：赋能→帮助、方法论→方法、抓手→切入点
- 过渡词普化：综上所述→总的来说、然而→不过

### 风格弱化
- 删除"我觉得""我认为"等主观表述
- 弱化权威语气：必须→需要、显然→可以看出
- 适当加入冗余和模糊词：可能、或许、在一定程度上

### 硬约束
- ✅ 字数变化 ±5%以内
- ✅ 不增加新观点或数据
- ✅ 保留引用、数字、专有名词
- ✅ 保留段落边界

## 📁 项目结构

```
reduce-aigc-rate/
├── SKILL.md                    # Skill 主文档（Claude Code 加载用）
├── README.md                   # 项目说明（你正在看的）
├── scripts/
│   ├── extract_chapters.py     # 章节提取
│   └── write_back.py           # 修改回写
└── examples/
    └── sample_original.docx    # 示例文档
```

## ❓ FAQ

**Q: 会不会改变我的排版和格式？**

A: 标题、字体、字号、表格都会保留。段落级别的格式（如加粗、斜体）在当前版本中会丢失，后续会改进。

**Q: 支持哪些 Word 版本？**

A: 仅支持 `.docx`（Office 2007及之后版本）。不支持旧版 `.doc` 格式。

**Q: 改写后AIGC率能降多少？**

A: 取决于原文的AI痕迹程度，一般可降低 30-60 个百分点。建议改写后用检测工具验证。

**Q: 会改变我的论文内容吗？**

A: 只调整表达方式，不改变核心观点、数据、引用。字数控制在 ±5% 以内。

**Q: 我没有 Python 怎么办？**

A: 参考上方的安装指引。后续会提供 Node.js 版本（无需额外安装）。

## 🤝 贡献

欢迎提 Issue 和 PR！

**贡献方向：**
- 改进改写规则（增加更多同义词替换表）
- 支持更多文档格式（.doc, .md, LaTeX）
- Node.js 版实现
- 更好的格式保留

## 📄 License

MIT © 2025

---

<details>
<summary>English Description</summary>

A Claude Code Skill for reducing AI-generated content (AIGC) detection rates in academic papers. Automatically extracts chapters from Word documents, rewrites each chapter to remove AI traces, and writes the results back — with automatic backup.

**Features:** Chapter detection via heading styles, 11 rewriting rules (restructure sentences, replace AI-favorite words, weaken professional tone), format preservation, automatic backup.

**Requirements:** Python 3.8+ with `python-docx`.

**Usage:** `/reduce-aigc-rate help me reduce the AI detection rate of my-paper.docx`

</details>

## ⚠️ 免责声明

本工具仅供学术诚信范围内的合法用途使用，包括但不限于：个人学习笔记的语言润色、原创论文的表达优化、以及合法合规的文本改写需求。用户应确保所处理的文档内容不侵犯他人知识产权，且改写行为符合所在机构或出版方的学术规范。使用者需自行承担因不当使用本工具而产生的一切责任。**禁止将本工具用于抄袭、剽窃、学术造假或其他任何违反学术道德或法律法规的用途。**
