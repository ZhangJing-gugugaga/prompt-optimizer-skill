# Hermes 集成指南

## 安装

```bash
# 克隆到 Hermes 的 skills 目录
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git \
  ~/.hermes/skills/productivity/prompt-optimizer
```

Hermes 自动识别。触发词：`优化提示词`、`改进prompt`、`提示词太模糊`、`AI输出不稳定`、`自动优化提示词`、`prompt-optimizer mcp`、`MCP提示词优化`。

---

## 使用示例

### 基础用法

```
用户：帮我优化这个提示词：帮我写个报告

Hermes 加载 prompt-optimizer skill，走四阶段流程：
阶段一：需求分析 → 阶段二：结构规划 → 阶段三：提示词生成 → 阶段四：质量验证

输出：结构化技术报告模板（6 章节，含格式/风格/字数要求）
```

### 复杂任务分解

```
用户：我有段需求很乱，帮我拆成可执行的步骤：[需求]

→ 用 user-prompt-planning 模板
→ 输出：准备/执行/验证三阶段 + 依赖关系 + 风险点 + 成功标准
```

### 提示词迭代

```
用户：这个提示词用了几次效果不稳定，帮我改进：[提示词]

→ 用 iterate-prompt 模板
→ 输出：原始分析 → 3 个迭代版本 → 对比表 → 推荐建议
```

---

## 文件结构

```
~/.hermes/skills/productivity/prompt-optimizer/
├── SKILL.md                           ← 主 skill 文件
├── README.md                          ← 项目说明
├── references/                        ← 方法论和参考资料
│   ├── methodology.md                 ← 核心原则 + 结构模板
│   ├── evaluation-dimensions.md       ← 4 维度评分体系
│   ├── common-patterns.md             ← 10 种提示词模式
│   ├── cron-prompt-optimization-example.md ← 完整实战案例
│   └── mcp-integration.md             ← MCP 工具集成
├── templates/                         ← 按场景选用的模板
│   ├── user-prompt-optimize.md
│   ├── system-prompt-optimize.md
│   ├── iterate-prompt.md
│   └── user-prompt-planning.md
├── scripts/
│   └── validate-prompt.md             ← 质量验证脚本
└── integrations/                      ← 其他 CLI 集成指南
    ├── claude-code.md
    ├── codex.md
    ├── gemini-cli.md
    ├── agy-cli.md
    └── hermes.md
```

---

## Skill 调用链

当 Hermes 触发本 skill 时，内部调用顺序：

1. 加载 `SKILL.md` → 获取角色定义和工作流程
2. 按优化类型选模板：
   - 系统提示词 → `references/methodology.md`
   - 用户提示词 → `templates/user-prompt-optimize.md`
   - 步骤化规划 → `templates/user-prompt-planning.md`
   - 迭代优化 → `templates/iterate-prompt.md`
3. 生成后验证 → `scripts/validate-prompt.md`
4. 可选 A/B 测试 → 对比原始版和优化版
