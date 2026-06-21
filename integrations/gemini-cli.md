# Gemini CLI 集成指南

> ⚠️ **注意**：Google 已于 2026-06-18 停止 Gemini CLI 的请求服务，推荐迁移到 [Agy CLI（Antigravity）](./agy-cli.md)。以下内容供仍在使用旧版的用户参考。

## 安装

Gemini CLI 不支持 skill 目录机制，需要通过 `GEMINI_SYSTEM_MD` 或 `GEMINI.md` 集成。

### 方式一：系统提示词覆盖（推荐）

将核心方法论写入系统提示词文件：

```bash
# 导出默认系统提示词作为模板
GEMINI_WRITE_SYSTEM_MD=~/.gemini/system.md gemini

# 把 prompt-optimizer 的核心原则追加进去
cat >> ~/.gemini/system.md << 'EOF'

## 提示词优化能力
当用户要求优化提示词时，你是一位 Prompt 工程架构师。执行四阶段流程：

### 阶段一：需求分析
识别核心意图、缺失信息、优化类型（系统/用户/迭代）

### 阶段二：结构规划
角色定位 → 目标设定 → 步骤分解 → 约束识别 → 输出定义

### 阶段三：提示词生成
按 5 条检查清单生成：
1. 角色明确 2. 对象具体 3. 结构清晰 4. 风格约束 5. 限制清楚

### 阶段四：质量验证
从完整性/清晰性/可执行性/结构性四个维度打分（1-5分）
EOF

# 启用自定义系统提示词
GEMINI_SYSTEM_MD=~/.gemini/system.md gemini
```

持久化设置（项目级）：

```bash
# 在项目 .gemini/.env 中添加
echo "GEMINI_SYSTEM_MD=~/.gemini/system.md" >> .gemini/.env
```

### 方式二：GEMINI.md 上下文文件

在项目根目录或 `~/.gemini/GEMINI.md` 中添加：

```markdown
# 提示词优化规范
优化提示词时遵循四阶段方法论：
1. 需求分析 → 2. 结构规划 → 3. 提示词生成 → 4. 质量验证
完整参考：references/methodology.md
```

### 方式三：自定义命令

创建 `~/.gemini/commands/optimize-prompt.toml`：

```toml
description = "优化模糊提示词为结构化版本"

prompt = """
你是 Prompt 工程架构师。对以下提示词执行四阶段优化：

1. 需求分析：识别核心意图、发现缺失信息、判断优化类型
2. 结构规划：角色定位 → 目标设定 → 步骤分解 → 约束识别 → 输出定义
3. 提示词生成：按 5 条检查清单生成（角色/对象/结构/风格/限制）
4. 质量验证：完整性/清晰性/可执行性/结构性 四维度评估

用 5 条检查清单快速过一遍：
| 1 | 角色明确 | AI 知道它是谁吗？ |
| 2 | 对象具体 | 知道要处理什么吗？ |
| 3 | 结构清晰 | 输出格式要求明确吗？ |
| 4 | 风格约束 | 语言风格指定了吗？ |
| 5 | 限制清楚 | 边界和禁止项说清了吗？ |

请优化以下提示词：{{args}}
"""
```

使用：`/optimize-prompt 帮我写个报告`

---

## 使用示例

```
$ GEMINI_SYSTEM_MD=1 gemini
|⌐■_■| > 帮我优化这个提示词：帮我写一篇公众号文章

Gemini 按四阶段流程输出结构化版本。
```

---

## 迁移建议

Gemini CLI 已停止服务。建议迁移到 [Agy CLI（Antigravity）](./agy-cli.md)，它原生支持 `SKILL.md` 格式，安装更简单：

```bash
git clone https://github.com/ZhangJing-gugugaga/prompt-optimizer-skill.git \
  .agents/skills/prompt-optimizer
```
