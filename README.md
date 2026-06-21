# Prompt Optimizer

> 把模糊的"帮我写个东西"变成 AI 真正能执行的专家级提示词。
>
> 一套方法论 + 模板 + 评估体系，通用于所有主流 AI CLI 代理工具。

## 支持平台

| 平台 | 安装命令 | 详细指南 |
|------|---------|---------|
| **Claude Code** | `git clone … ~/.claude/skills/prompt-optimizer` | [集成指南](integrations/claude-code.md) |
| **Codex CLI** | `git clone … ~/.codex/skills/prompt-optimizer` | [集成指南](integrations/codex.md) |
| **Agy CLI** | `git clone … .agents/skills/prompt-optimizer` | [集成指南](integrations/agy-cli.md) |
| **Hermes** | `git clone … ~/.hermes/skills/productivity/prompt-optimizer` | [集成指南](integrations/hermes.md) |
| **Gemini CLI** | 通过 `GEMINI_SYSTEM_MD` 或自定义命令 | [集成指南](integrations/gemini-cli.md) |

> 💡 所有平台共用同一套文件，放到对应目录即可。各平台的详细配置说明见 `integrations/` 目录。

## 快速开始

### 1. 安装

选你的工具，一行命令：

```bash
# Claude Code
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git ~/.claude/skills/prompt-optimizer

# Codex CLI
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git ~/.codex/skills/prompt-optimizer

# Agy CLI (Antigravity)
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git .agents/skills/prompt-optimizer

# Hermes
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git ~/.hermes/skills/productivity/prompt-optimizer
```

### 2. 使用

对 AI 说一句人话即可：

> "帮我优化这个提示词：帮我写个报告"

AI 会自动加载方法论，走完四阶段流程，返回结构化版本。

---

## 它能做什么

| 场景 | 例子 |
|------|------|
| **用户提示词优化** | "帮我写个报告" → 6 章节结构化技术报告模板 |
| **系统提示词优化** | "你是客服" → 完整客服专家角色定义（职责/原则/禁止/升级） |
| **复杂任务分解** | 一段混乱的需求 → 分阶段执行计划（准备/执行/验证） |
| **提示词迭代** | 基于反馈改进现有提示词，产出 3 个版本并对比 |
| **质量评估** | 用 4 维度评分体系给现有提示词打分，指出问题 |

---

## 核心工作流程

```
模糊需求 → [阶段一: 需求分析] → [阶段二: 结构规划] → [阶段三: 提示词生成] → [阶段四: 质量验证] → 结构化提示词
```

### 阶段一：需求分析
- 理解原始需求
- 识别核心意图
- 发现缺失信息（上下文、约束、细节）
- 判断优化类型：系统提示词 / 用户提示词 / 迭代优化

### 阶段二：结构规划
- 角色定位 → 目标设定 → 步骤分解 → 约束识别 → 输出定义

### 阶段三：提示词生成
按场景选模板：

| 需求 | 用的模板 |
|------|---------|
| 优化一个任务描述 | `templates/user-prompt-optimize.md` |
| 定义一个 AI 角色 | `references/methodology.md` 系统提示词结构 |
| 把复杂任务拆成步骤 | `templates/user-prompt-planning.md` |
| 改进现有提示词 | `templates/iterate-prompt.md` |

### 阶段四：质量验证
用 `scripts/validate-prompt.md` 从 4 个维度打分（每项 1-5）：
- **完整性**：该有的都有吗？
- **清晰性**：有没有歧义？
- **可执行性**：AI 能直接照做吗？
- **结构性**：逻辑清楚吗？

综合分 ≥ 4.5 为 A 级（可直接使用）。

---

## 快速检查清单

优化完任何提示词，用这 5 条过一遍：

| # | 检查项 | 问自己 |
|---|--------|--------|
| 1 | 角色明确 | AI 知道它是谁吗？ |
| 2 | 对象具体 | 知道要处理什么吗？ |
| 3 | 结构清晰 | 输出格式要求明确吗？ |
| 4 | 风格约束 | 语言风格指定了吗？ |
| 5 | 限制清楚 | 边界和禁止项说清了吗？ |

---

## 文件结构

```
prompt-optimizer/
├── README.md                              ← 你正在看的这个
├── SKILL.md                               ← 主文件：角色定义、工作流程、检查清单、FAQ
├── references/                            ← 方法论与参考资料
│   ├── methodology.md                     ← 4 条核心原则 + 系统/用户提示词结构模板
│   ├── evaluation-dimensions.md           ← 4 维度评分体系（完整性/清晰性/可执行性/结构性）
│   ├── common-patterns.md                 ← 10 种提示词模式（角色扮演/分步/对比…）
│   ├── cron-prompt-optimization-example.md ← 完整实战案例：cron 任务 prompt 优化
│   └── mcp-integration.md                 ← MCP 工具集成（npx/Docker，可选增强）
├── templates/                             ← 按场景选用的提示词模板
│   ├── user-prompt-optimize.md            ← 用户提示词优化模板
│   ├── system-prompt-optimize.md          ← 系统提示词优化模板
│   ├── iterate-prompt.md                  ← 迭代优化模板（3 版本对比）
│   └── user-prompt-planning.md            ← 步骤化规划模板（含依赖/风险/成功标准）
├── scripts/
│   └── validate-prompt.md                 ← 验证脚本：4 维度打分 + 问题清单 + 改进建议
└── integrations/                          ← 各平台集成指南
    ├── claude-code.md                     ← Claude Code 配置
    ├── codex.md                           ← Codex CLI 配置
    ├── agy-cli.md                         ← Agy CLI (Antigravity) 配置
    ├── gemini-cli.md                      ← Gemini CLI 配置（旧版）
    └── hermes.md                          ← Hermes 配置
```

---

## A/B 测试

优化不是一次到位的。建议准备两个版本用相同输入对比，从四个维度评估：

1. **准确性** — 是否回答了问题
2. **完整性** — 是否覆盖了要点
3. **一致性** — 多次输出是否稳定
4. **可用性** — 输出能否直接使用

---

## MCP 工具集成（可选）

如果有 `@nicepkg/prompt-optimizer` MCP 包可用，可以用 3 个工具自动优化：

- `optimize-user-prompt` — 优化用户级提示词
- `optimize-system-prompt` — 优化系统级提示词
- `iterate-prompt` — 迭代优化现有提示词

> ⚠️ 截至 2026-06-20，npm 包 `@nicepkg/prompt-optimizer` 返回 404。**手动使用本 skill 的方法论 + 模板效果一致。**

详见 `references/mcp-integration.md`。

---

## 资产管理

好的提示词是可复用资产，建议：

- **版本控制**：记录每次优化的版本和效果
- **分类存储**：按用途分（写作、代码、客服、cron）
- **命名规范**：`用途_版本_日期`（如 `公众号写作_v2_202606`）或 `角色_场景`（如 `代码审查员_安全检查`）
- **定期复盘**：每月检查效果，更新过时的

---

## 实战案例

`references/cron-prompt-optimization-example.md` 记录了一个完整的 cron 任务 prompt 优化过程——从一段混乱的飞书自动化需求，到可执行的结构化提示词。包含四个阶段的完整记录和最终产出对比。

---

## 版本

- **v1** (2026-06-21)：合并 `prompt-optimization` + `prompt-optimizer` + `prompt-optimizer-mcp` 三个 skill 为统一版本。新增 5 平台集成指南。保留完整方法论、10 种模式、4 套模板、MCP 集成文档、实战案例。

---

## 许可

MIT
