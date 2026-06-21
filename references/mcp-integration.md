# MCP 工具集成 — prompt-optimizer

## 概述

`@nicepkg/prompt-optimizer` 提供 3 个 MCP 工具，可以自动优化提示词。作为本 skill 的可选增强——如果 MCP 包可用，用工具自动优化；不可用时，用 skill 的方法论 + 模板手动优化，效果一致。

## ⚠️ 当前状态（2026-06-20）

npm 包 `@nicepkg/prompt-optimizer` 返回 404，不可用。以下为备用方案：

### 方式1：npx 直接运行（如果包恢复可用）

在 Hermes config.yaml 中添加：

```yaml
mcp_servers:
  prompt-optimizer:
    command: npx
    args: ["-y", "@nicepkg/prompt-optimizer"]
    env:
      OPENAI_API_KEY: "your-api-key"
      OPENAI_BASE_URL: "https://api.openai.com/v1"
      OPENAI_MODEL: "gpt-4o"
```

### 方式2：Docker 部署

```bash
docker run -d -p 8081:80 \
  -e VITE_OPENAI_API_KEY=your_key \
  -e ACCESS_USERNAME=admin \
  -e ACCESS_PASSWORD=your_password \
  --restart unless-stopped \
  --name prompt-optimizer \
  linshen/prompt-optimizer
```

然后配置 MCP：
```yaml
mcp_servers:
  prompt-optimizer:
    url: "http://localhost:8081/mcp"
```

## 可用工具

### 1. optimize-user-prompt
优化用户级提示词，让模糊需求变成清晰指令。

参数：
- `prompt`: 原始提示词（必填）
- `model`: 使用的模型（可选，默认用配置的模型）

### 2. optimize-system-prompt
优化系统级提示词，完善角色设定和行为规范。

参数：
- `prompt`: 原始系统提示词（必填）
- `model`: 使用的模型（可选）

### 3. iterate-prompt
迭代优化现有提示词，根据具体反馈调整。

参数：
- `prompt`: 要迭代的提示词（必填）
- `instruction`: 迭代指令（必填）
- `model`: 使用的模型（可选）

## 支持的模型

由于使用 OpenAI 兼容 API，支持：

| 提供商 | Base URL | 推荐模型 |
|--------|----------|---------|
| OpenAI | https://api.openai.com/v1 | gpt-4o |
| DeepSeek | https://api.deepseek.com/v1 | deepseek-chat |
| 小米 MiMo | https://api.xiaomimimo.com/v1 | mimo-v2-pro |
| 本地 Ollama | http://localhost:11434/v1 | llama3 |

## 使用场景

### 场景1：优化写作提示词
```
使用optimize-user-prompt优化：帮我写一篇关于AI的公众号文章
```
→ 输出结构化版本（角色、要求、格式、风格）

### 场景2：完善系统提示词
```
使用optimize-system-prompt优化：你是客服
```
→ 输出完整的客服系统提示词（职责、原则、禁止行为）

### 场景3：迭代优化
```
使用iterate-prompt，把提示词改得更口语化：[原始提示词]
```
→ 输出口语化版本
