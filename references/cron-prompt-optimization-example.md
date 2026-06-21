# Cron Prompt Optimization Example (2026-06-20)

## Original Prompt (user-provided)
```
我现在需要你再每天凌晨五点自动把前一天所有的飞书消息全部汇总判断，如果我能立即使用的，你就直接配置好（skills，MCP，hooks，plugins），并且开claude至少检查三次是否配置成功，每一次检测都不能使用一个claude，其次，如果飞书消息对我有用但是我无法立即使用的（github热点，需要用户手动进行登录或者重要操作之类的工具或者内容），你不进行配置，你给使用计划；如果是论文相关，就直接使用llm-wiki编译进知识库

以上，汇总润色完后以md形式写入HIMRA里，调用hooks使用C:\Users\Public\Desktop\MarkText.lnk 打开汇总md文档
```

## Issues with Original
1. **分类标准模糊**："能立即使用" vs "有用但无法立即使用" — 判定条件不明确
2. **验证流程不具体**："开claude至少检查三次" — 用什么工具？检查什么方面？
3. **输出格式缺失**：日报长什么样？包含哪些章节？
4. **环境信息缺失**：lark-cli 路径、chat_id、编辑器路径散落在文本中
5. **错误处理缺失**：某个步骤失败怎么办？

## Optimization Process

### Phase 1: Intent Analysis
- 核心意图：自动化飞书消息分类处理 + 生成日报
- 缺失信息：分类判定标准、验证方法、输出格式、错误处理

### Phase 2: Structure Planning
- 角色：自动化运维与知识管理专家
- 四阶段流程：获取 → 分类处理 → 生成日报 → 打开文档
- 约束：UTC+8 时区、独立 Claude 验证、MarkText 打开

### Phase 3: Prompt Generation
- 环境信息表格化
- 三类分类标准明确化（A=自动配置、B=使用计划、C=论文编译）
- 验证流程具体化（delegate_task tasks 模式，3 个独立实例）
- 输出格式模板化（4 个章节的 Markdown 模板）
- 错误处理表格化（6 种场景 + 处理方式）

### Phase 4: Quality Validation
- 完整性：5/5（覆盖所有原始需求 + 补充了验证、输出、错误处理）
- 清晰性：5/5（分类标准无歧义）
- 可执行性：5/5（AI 可直接执行，无需额外解释）
- 结构性：5/5（四阶段流程清晰）

## Key Optimization Patterns Applied

1. **表格化环境信息**：避免散落在文本中
2. **明确分类标准**：用判定标准 + 处理操作的结构
3. **具体化验证方法**：指定工具（delegate_task）、实例数量（3）、检查维度（文件/依赖/功能）
4. **模板化输出格式**：提供完整的 Markdown 模板
5. **错误处理矩阵**：6 种场景 × 处理方式
6. **约束条件清单化**：必须满足 + 禁止操作
