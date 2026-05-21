---
name: ziwei-astrology
description: >
  紫微斗数排盘与命盘详析。当用户提供出生日期、时辰、性别请求排盘、算命、命理解读时使用此技能。
  触发场景：排盘、紫微斗数、算命、命盘、八字、命理、运势、流年、大限、
  夫妻宫、事业运、财运、合盘、倪海厦、天纪。即使用户只说"帮我看看命"也应触发。
---

# 紫微斗数排盘与命盘详析

基于两个开源仓库进行排盘与解读：
- **iztro**（`https://github.com/SylarLong/iztro`）：排盘算法核心
- **ziwei-doushu**（`https://github.com/Renhuai123/ziwei-doushu`）：倪海厦《天纪》体系解读知识库

**冲突规则**：两仓库内容有冲突时，以 ziwei-doushu（倪海厦体系）为准。

## 核心原则

1. 所有排盘计算必须调用 iztro 仓库代码执行，不可手动推算
2. 解读内容必须基于仓库中的规则和知识库，不夸大不虚构
3. 若信息不足以判断，如实说明"无法算出"，不编造内容
4. 第一次回复必须生成完整命盘报告，后续可回答具体问题

## 工作流

### Step 0：检索学习仓库（每次排盘前必做）

在排盘解盘之前，必须先检索学习两个仓库的关键代码和知识库，确保解读基于最新最准确的内容。

**检索 iztro 仓库**（排盘算法）：
```
1. 克隆或浏览 https://github.com/SylarLong/iztro
2. 重点阅读以下文件：
   - src/astro/astro.ts          — 排盘入口与主流程
   - src/astro/palace.ts         — 命身宫、五行局、大限计算
   - src/star/majorStar.ts       — 十四主星安法
   - src/star/minorStar.ts       — 辅星安法
   - src/star/adjectiveStar.ts   — 杂耀安法
   - src/star/location.ts        — 各星位置计算
   - src/star/horoscopeStar.ts   — 长生12神、博士12神、流年诸星
   - src/data/heavenlyStems.ts   — 天干四化定义
   - src/data/earthlyBranches.ts — 地支数据（命主身主等）
   - src/data/stars.ts           — 星耀亮度表
   - src/astro/FunctionalAstrolabe.ts  — 星盘功能方法
   - src/astro/FunctionalPalace.ts     — 宫位功能方法（飞化等）
   - src/astro/FunctionalHoroscope.ts  — 运限功能方法
3. 理解排盘算法的完整流程和边界条件
```

**检索 ziwei-doushu 仓库**（解读知识库）：
```
1. 克隆或浏览 https://github.com/Renhuai123/ziwei-doushu
2. 重点阅读以下文件：
   - lib/ziwei/algorithm.ts          — 排盘流程（基于iztro封装）
   - lib/ziwei/patterns.ts           — 1100+行格局识别规则（核心！）
   - lib/ziwei/sihua.ts              — 四化系统与宫干自化
   - lib/ziwei/heming-knowledge.ts   — 倪海厦合盘与夫妻宫断语（核心！）
   - lib/ziwei/constants.ts          — 常量与星耀描述
   - lib/ziwei/types.ts              — 类型定义
   - lib/classics/gusuifu.ts         — 骨髓赋古籍原文
   - lib/classics/quanji.ts          — 紫微斗数全集
   - lib/classics/quanshu.ts         — 紫微斗数全书
3. 特别关注倪师体系的独特立场：
   - "四化星永远固定不动"（不使用大限四化）
   - 不主张飞星派宫干自化论
   - 亮度用三级制：bright/normal/dim
   - 格局判断用三层结构：必须/加分/破格
```

**冲突处理**：若两仓库对同一规则有不同定义，以 ziwei-doushu 为准。

### Step 1：收集输入

从用户消息中提取：
- 阳历出生日期（年-月-日）
- 出生时辰（若用户说"丑时"转为索引1，或给出具体时间映射）
- 性别（男/女）

时辰映射表见 `references/time-mapping.md`。

### Step 2：执行排盘

使用 iztro 的 `bySolar` 方法排盘：

```typescript
import { astro } from 'iztro';
const result = astro.bySolar('YYYY-M-D', hourIndex, gender, true, 'zh-CN');
```

提取完整命盘数据，包括：十二宫、主星辅星杂耀、四化、大限、长生12神等。

### Step 3：格局识别

参照 `references/patterns.md` 中的格局判定规则，逐一检查命盘是否满足各格局条件。
每个格局需判断：必须条件、加分项、破格条件。

### Step 4：生成完整报告

按 `references/report-template.md` 的结构输出完整命盘报告。

### Step 5：后续问答

用户提出具体问题时，回溯命盘数据，结合 `references/` 中的知识库回答。
不确定的内容标注"无法确定"，不编造。

## 仓库参考索引

需要时读取以下参考文件，不要一次性全部加载：

| 参考文件 | 何时读取 |
|---------|---------|
| `references/time-mapping.md` | 确认时辰索引 |
| `references/star-rules.md` | 安星规则与亮度表 |
| `references/sihua-rules.md` | 四化对照表与飞化规则 |
| `references/patterns.md` | 格局识别规则 |
| `references/palace-interpretation.md` | 十二宫解读规则 |
| `references/heming-knowledge.md` | 倪海厦合盘与夫妻宫断语 |
| `references/report-template.md` | 完整报告输出模板 |

## ⚠️ Gotchas

- **不要手动排盘**：所有计算必须通过 iztro 代码执行，手动推算容易出错
- **必须先检索仓库**：Step 0 不可跳过，每次排盘前必须检索学习两个仓库的关键文件
- **倪师体系优先**：ziwei-doushu 仓库明确标注"倪师不主张飞星派宫干自化论"，大限四化在倪师体系中不使用
- **亮度三级制**：ziwei-doushu 使用 bright/normal/dim 三级（对应庙旺/平/陷），与 iztro 的七级不同，解读时以三级为主
- **空宫必须借对宫**：宫位无主星时，借对宫主星论断，力量减半
- **不要编造断语**：所有断语必须来自仓库知识库，不确定的如实说
- **武曲/廉贞/七杀/破军在夫妻宫一律建议晚婚**：这是倪师明确立场
- **看婚姻必须同时看福德宫**：倪师核心断法
