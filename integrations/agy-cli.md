# Agy CLI（Antigravity）集成指南

## 什么是 Agy？

Agy（`agy`）是 Google 在 2026 年 I/O 大会上发布的新一代终端 AI 编程代理，是 Gemini CLI 的继任者。它原生支持 `SKILL.md` 格式的 skill 系统。

## 安装

### 方式一：作为 Skill 安装（推荐）

```bash
# 项目级安装
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git \
  .agents/skills/prompt-optimizer

# 全局安装
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git \
  ~/.gemini/config/skills/prompt-optimizer
```

Agy 会自动发现 skill。说 `优化提示词`、`改进prompt`、`提示词太模糊` 等关键词时自动激活。

也可以手动链接：

```bash
# 在 agy 交互模式中
/skills link ~/.gemini/config/skills/prompt-optimizer
/skills reload
```

### 方式二：通过 AGENTS.md 引用

在项目根目录的 `AGENTS.md` 中添加：

```markdown
# 提示词优化规范
当用户要求优化提示词时，使用 prompt-optimizer skill 的方法论：
- 四阶段流程：需求分析 → 结构规划 → 提示词生成 → 质量验证
- 四维度评估：完整性 / 清晰性 / 可执行性 / 结构性
- 五条检查清单：角色 / 对象 / 结构 / 风格 / 限制

Skill 位置：.agents/skills/prompt-optimizer/
```

> 注：如果同时存在 `AGENTS.md` 和 `GEMINI.md`，Agy 优先读取 `AGENTS.md`。

### 方式三：Headless 模式

在 CI 或脚本中使用：

```bash
# 直接让 agy 加载 skill 优化提示词
agy -p "加载 prompt-optimizer skill，优化这个提示词：帮我写个报告" \
  --output-format stream-json \
  --non-interactive
```

---

## 使用示例

### 交互模式

```
$ agy -i
> 帮我优化这个提示词：帮我写一篇公众号文章

Agy 自动加载 prompt-optimizer skill，输出：
请撰写一篇面向[目标读者]的公众号文章。

要求：
1. 开头用一个真实场景引入
2. 分3-4个小节，每节一个核心观点
3. 每个观点配一个具体案例
4. 语言口语化，每段不超过4行
5. 结尾给出可执行建议
```

### 使用 skill 中的模板

```
> 用 prompt-optimizer 的 iterate-prompt 模板，帮我迭代这个提示词：[你的提示词]
→ 输出 3 个迭代版本 + 对比评分表
```

---

## 与 Agy 原生功能的配合

| prompt-optimizer 功能 | Agy 对应机制 |
|----------------------|-------------|
| 系统提示词优化 | `AGENTS.md` 项目规则文件 |
| 用户提示词优化 | 对话中 `-p` 或交互模式任务描述 |
| Skill 管理 | `.agents/skills/` 目录，`/skills` 命令 |
| 资产管理 | Git 版本控制 + `.agents/` 目录 |
| A/B 测试 | 多个 `agy -p` 调用对比输出 |
| MCP 集成 | `.agents/mcp_config.json` |

---

## 注意事项

- Agy 是 Gemini CLI 的继任者，旧版 Gemini CLI 已于 2026-06-18 停止服务。
- `AGENTS.md` 优先级高于 `GEMINI.md`（如果两者同时存在）。
- Skill 的 YAML frontmatter 中 `name`、`description`、`triggers` 字段 Agy 完全支持。
- 如需 MCP 自动优化功能，在 `.agents/mcp_config.json` 中配置（参考 `references/mcp-integration.md`）。
