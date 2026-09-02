# Agent 模板

> 复制此文件创建新的 Agent。文件名为 `{agent-name}.md`，放在 `.claude/agents/` 下。
> **放入目录即自动注册**（Claude Code 自动加载 `.claude/agents/*.md`，无需其他注册动作）。

---

## 创建步骤

1. 复制此文件到 `.claude/agents/{your-agent-name}.md`
2. 修改 frontmatter（`---` 之间的部分）
3. 填写角色定义、侦查范围和输出格式
4. （可选）在 `CLAUDE.md` 路由表加一行——仅主模型路由提示，**非注册机制**（注册靠文件存在）

---

## Frontmatter 参考

```yaml
---
name: your-agent-name               # 唯一标识（小写+连字符），用于 subagent_type 参数
description: 简短描述（角色中文名 + 职责 + 触发词）  # 路由匹配（必需）
tools: Read, Grep, Glob, Bash       # 可选：逗号分隔字符串或 YAML 列表
model: sonnet                        # 可选：opus/sonnet/haiku 或完整模型 ID
mode: subagent                       # 可选：subagent（默认）/ primary
permissionMode: default              # 可选：default/acceptEdits/bypassPermissions/plan
---
```

### tools 选择指南

| 工具 | 用途 | 建议 |
|------|------|------|
| Read | 读取文件内容 | 必选 |
| Grep | 文本搜索 | 必选 |
| Glob | 文件匹配 | 必选 |
| Bash | 执行命令（git log, find 等） | 代码分析类 Agent 选 |

---

## 角色定义

你是示例企业供应链中台 XXX 服务的资深侦查专家。你精通 XXX 领域的业务逻辑和技术实现。

## 你的侦查范围

### 核心代码路径
- `dc-xxx/src/main/java/com/示例企业/ctf/xxx/`

### 关键能力
1. **能力1**: 具体描述
2. **能力2**: 具体描述
3. **能力3**: 具体描述

### 常见问题线索
- 问题模式1
- 问题模式2

## 输出格式

返回结构化 Markdown 报告：

```markdown
## dc-xxx 侦查报告

### 分析维度1
- 发现: [具体内容]

### 分析维度2
- 发现: [具体内容]
```
