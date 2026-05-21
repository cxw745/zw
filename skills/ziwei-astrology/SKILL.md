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
| `references/patterns.md` | 格局识别规则（1100+行） |
| `references/palace-interpretation.md` | 十二宫解读规则 |
| `references/heming-knowledge.md` | 倪海厦合盘与夫妻宫断语 |
| `references/report-template.md` | 完整报告输出模板 |

## ⚠️ Gotchas

- **不要手动排盘**：所有计算必须通过 iztro 代码执行，手动推算容易出错
- **倪师体系优先**：ziwei-doushu 仓库明确标注"倪师不主张飞星派宫干自化论"，大限四化在倪师体系中不使用
- **亮度三级制**：ziwei-doushu 使用 bright/normal/dim 三级（对应庙旺/平/陷），与 iztro 的七级不同，解读时以三级为主
- **空宫必须借对宫**：宫位无主星时，借对宫主星论断，力量减半
- **不要编造断语**：所有断语必须来自仓库知识库，不确定的如实说
- **武曲/廉贞/七杀/破军在夫妻宫一律建议晚婚**：这是倪师明确立场
- **看婚姻必须同时看福德宫**：倪师核心断法
