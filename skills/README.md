# ziwei-astrology Skill

紫微斗数排盘与命盘详析 AI Skill —— 基于倪海厦《天纪》体系，融合 [iztro](https://github.com/SylarLong/iztro) 排盘引擎与 [ziwei-doushu](https://github.com/Renhuai123/ziwei-doushu) 知识库。

## 功能

- 🌟 输入出生日期、时辰、性别，自动排出完整紫微斗数命盘
- 📖 基于倪海厦《天纪》体系进行详尽解读（格局、四化、大限、流年等）
- 💍 夫妻宫断语、合盘分析（倪师核心断法）
- 🏥 健康提示、事业财运建议
- 🔍 排盘前自动检索学习两个仓库的最新代码与知识库

## 依赖仓库

| 仓库 | 用途 | 地址 |
|------|------|------|
| iztro | 排盘算法核心 | https://github.com/SylarLong/iztro |
| ziwei-doushu | 倪海厦《天纪》体系解读知识库 | https://github.com/Renhuai123/ziwei-doushu |

**冲突规则**：两仓库内容有冲突时，以 ziwei-doushu（倪海厦体系）为准。

## 安装

### Claude Code

```bash
# 个人全局安装
mkdir -p ~/.claude/skills
cp -r skills/ziwei-astrology ~/.claude/skills/ziwei-astrology

# 项目级安装
mkdir -p .claude/skills
cp -r skills/ziwei-astrology .claude/skills/ziwei-astrology
```

### Trae

```bash
# Trae 使用项目级 skills 目录
mkdir -p .trae/skills
cp -r skills/ziwei-astrology .trae/skills/ziwei-astrology
```

或在 Trae 设置中指定 skills 路径指向本仓库的 `skills/ziwei-astrology` 目录。

### Cursor

```bash
# Cursor 使用项目级 .cursor/skills 目录
mkdir -p .cursor/skills
cp -r skills/ziwei-astrology .cursor/skills/ziwei-astrology
```

### Codex (OpenAI)

```bash
# Codex 使用项目级 .codex/skills 目录
mkdir -p .codex/skills
cp -r skills/ziwei-astrology .codex/skills/ziwei-astrology
```

### 通用方式

将 `skills/ziwei-astrology/` 目录复制到你所用 AI 工具的 skills 目录下即可。核心文件是 `SKILL.md`，AI 工具会自动识别 frontmatter 中的 `name` 和 `description` 来决定何时触发。

## 目录结构

```
skills/ziwei-astrology/
├── SKILL.md                          # 核心指令文件（AI 读取入口）
└── references/                       # 渐进式披露参考文档（按需加载）
    ├── time-mapping.md               # 时辰映射表
    ├── star-rules.md                 # 安星规则与亮度表
    ├── sihua-rules.md                # 四化对照表与飞化规则
    ├── patterns.md                   # 格局识别规则
    ├── palace-interpretation.md      # 十二宫解读规则
    ├── heming-knowledge.md           # 倪海厦合盘与夫妻宫断语
    └── report-template.md            # 完整报告输出模板
```

## 使用方法

安装后，直接对 AI 说：

- "帮我排盘，我是2002年8月25日丑时出生的男生"
- "帮我看看命盘，1990年6月15日酉时女"
- "紫微斗数排盘，1985年3月20日午时男"
- "帮我算算运势"

AI 会自动触发此 Skill，按以下流程执行：

1. **检索学习仓库** — 读取 iztro 和 ziwei-doushu 的关键文件
2. **收集输入** — 提取出生日期、时辰、性别
3. **执行排盘** — 调用 iztro 的 `bySolar` 方法
4. **格局识别** — 按三层结构（必须/加分/破格）判断格局
5. **生成报告** — 输出完整命盘详析
6. **后续问答** — 基于命盘数据回答具体问题

## 倪海厦体系核心立场

本 Skill 严格遵循倪海厦《天纪》体系，与飞星派有以下关键差异：

| 项目 | 倪师体系 | 飞星派 |
|------|---------|--------|
| 大限四化 | ❌ 不使用（"四化星永远固定不动"） | ✅ 使用大限宫干四化 |
| 宫干自化 | ❌ 不主张 | ✅ 常用 |
| 亮度分级 | 三级（庙旺/平/陷） | 七级（庙旺得利平不陷） |
| 格局判断 | 三层结构（必须/加分/破格） | 无统一标准 |
| 婚姻判断 | 必须同时看夫妻宫+福德宫 | 主要看夫妻宫 |

## 示例输出

见仓库中的示例命盘详析：
- [命盘详析_2002年8月25日丑时男.md](./命盘详析_2002年8月25日丑时男.md)
- [命盘详析_2002年8月25日子时男.md](./命盘详析_2002年8月25日子时男.md)

## 许可证

MIT License

## 致谢

- [iztro](https://github.com/SylarLong/iztro) — 紫微斗数排盘引擎
- [ziwei-doushu](https://github.com/Renhuai123/ziwei-doushu) — 倪海厦《天纪》体系知识库
- 倪海厦老师 — 《天纪》紫微斗数讲义
