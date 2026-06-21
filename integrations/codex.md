# OpenAI Codex CLI 集成指南

## 安装

### 方式一：作为 Skill 安装（推荐）

```bash
# 克隆到 Codex 的 skills 目录
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git \
  ~/.codex/skills/prompt-optimizer
```

Codex 会根据上下文自动激活 skill。当你说 `优化提示词`、`改进prompt`、`提示词太模糊` 等关键词时，Codex 会自动加载本 skill。

### 方式二：作为自定义 Prompt（交互模式）

将核心方法论设为自定义 prompt，在交互模式中通过 `/prompts:optimize` 调用：

```bash
mkdir -p ~/.codex/prompts
```

创建 `~/.codex/prompts/optimize.md`：

```markdown
你是 Prompt 工程架构师。对以下提示词执行四阶段优化：

## 阶段一：需求分析
- 识别核心意图和缺失信息
- 判断优化类型（系统提示词/用户提示词/迭代）

## 阶段二：结构规划
- 角色定位 → 目标设定 → 步骤分解 → 约束识别 → 输出定义

## 阶段三：提示词生成
按 5 条检查清单生成优化版本：
1. 角色明确 2. 对象具体 3. 结构清晰 4. 风格约束 5. 限制清楚

## 阶段四：质量验证
从完整性/清晰性/可执行性/结构性四个维度评估。

原始提示词：$ARGUMENTS
```

使用：`/prompts:optimize 帮我写个报告`

### 方式三：通过 AGENTS.md 引用

在 `~/.codex/AGENTS.md` 或项目 `AGENTS.md` 中添加：

```markdown
# 提示词优化规范
当用户要求优化提示词时，遵循以下方法论：
1. 需求分析：识别意图、缺失信息、优化类型
2. 结构规划：角色→目标→步骤→约束→输出
3. 提示词生成：按 5 条检查清单生成
4. 质量验证：完整性/清晰性/可执行性/结构性 四维度打分
完整方法论见：https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill
```

---

## 使用示例

### Codex 交互模式

```
$ codex
> 帮我优化这个提示词：帮我写一篇公众号文章

Codex 加载 prompt-optimizer skill，输出：
请撰写一篇面向[目标读者]的公众号文章。
要求：
1. 开头用一个真实场景引入
2. 分3-4个小节，每节一个核心观点
3. 每个观点配一个具体案例
4. 语言口语化，每段不超过4行
5. 结尾给出可执行建议
```

### Codex exec 模式

```bash
# 通过 developer_instructions 注入方法论
codex exec \
  --config developer_instructions="你是Prompt工程专家。优化用户提示词时遵循四阶段流程：需求分析→结构规划→生成→验证。" \
  "优化这个提示词：帮我写个报告"
```

---

## 与 Codex 原生功能的配合

| prompt-optimizer 功能 | Codex 对应机制 |
|----------------------|---------------|
| 系统提示词优化 | `model_instructions_file` 或 `AGENTS.md` |
| 用户提示词优化 | 对话 / `codex exec` 任务描述 |
| 自定义 prompt | `~/.codex/prompts/*.md` → `/prompts:name` |
| 资产管理 | `~/.codex/skills/` 目录，可 Git 版本控制 |
| A/B 测试 | 多个 `codex exec` 调用对比输出 |

---

## 注意事项

- Codex 的 `model_instructions_file` 是**完全替换**系统提示词，不是追加。如需保留默认行为，用 `AGENTS.md` 或 `developer_instructions` 更安全。
- 自定义 prompt（`/prompts:name`）仅在**交互模式**可用，`exec` 模式不支持。
- Skill 自动激活依赖上下文匹配，如果没自动加载，可以显式说"使用 prompt-optimizer skill"。
