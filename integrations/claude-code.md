# Claude Code 集成指南

## 安装

### 方式一：作为 Skill 安装（推荐）

```bash
# 克隆到 Claude Code 的 skills 目录
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git \
  ~/.claude/skills/prompt-optimizer

# 或者放到项目级 skills 目录
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git \
  .claude/skills/prompt-optimizer
```

Claude Code 会自动发现并加载这个 skill。触发词包括：`优化提示词`、`改进prompt`、`提示词太模糊`、`AI输出不稳定` 等。

### 方式二：作为 CLAUDE.md 引用

在项目的 `CLAUDE.md` 或全局 `~/.claude/CLAUDE.md` 中添加：

```markdown
## 提示词优化
优化提示词时，参考 Claude Code skill `prompt-optimizer` 的方法论：
- 四阶段流程：需求分析 → 结构规划 → 提示词生成 → 质量验证
- 四维度评估：完整性 / 清晰性 / 可执行性 / 结构性
- 五条检查清单：角色 / 对象 / 结构 / 风格 / 限制
```

### 方式三：手动调用

对 Claude Code 说：

> "加载 prompt-optimizer skill，帮我优化这个提示词：[你的提示词]"

---

## 使用示例

### 优化模糊的用户提示词

```
用户：帮我写个报告
→ Claude Code 加载 prompt-optimizer skill
→ 走四阶段流程
→ 输出：6 章节结构化技术报告模板
```

### 定义 AI 角色（系统提示词）

```
用户：你是客服
→ 输出：完整客服专家角色定义（职责/原则/禁止/升级条件）
```

### 迭代改进现有提示词

```
用户：我这个提示词效果不稳定，帮我改进：[提示词]
→ 输出 3 个迭代版本 + 对比表 + 推荐建议
```

---

## 与 Claude Code 原生功能的配合

| prompt-optimizer 功能 | Claude Code 对应机制 |
|----------------------|---------------------|
| 系统提示词优化 | `CLAUDE.md` 项目指令 |
| 用户提示词优化 | 对话中的任务描述 |
| A/B 测试 | 多个对话窗口对比 |
| 资产管理 | `.claude/skills/` 目录版本控制 |
| 质量验证 | `scripts/validate-prompt.md` |

---

## 文件结构说明

Claude Code skill 目录下的文件会被自动识别：

```
.claude/skills/prompt-optimizer/
├── SKILL.md              ← 主文件，Claude Code 自动加载
├── references/           ← 按需读取的参考文档
├── templates/            ← 提示词模板
├── scripts/              ← 验证脚本
└── integrations/         ← 其他 CLI 的集成指南（与本平台无关）
```
