---
name: 继续会话提示词模板
description: 压缩会话后恢复上下文的提示词模板，含已沉淀资产索引 + 待办清单
metadata:
  type: project
---

# 继续会话提示词模板

> 用途：会话压缩（/compact）或新会话时，用此模板恢复任务上下文。
> 触发：用户表达压缩意图（"压缩"/"继续会话"）时，AI 自动在回复末尾输出对应任务的继续提示词。
> 依据：`research/上下文过长弊端与大模型影响.md`（96% context 注意力衰减实证，需及时压缩）

## 模板

```markdown
# 继续会话 — {任务名}

## 任务上下文
{一句话任务背景：在排查/开发什么，当前状态}

## 已完成（勿重做）
- **根因**：{一句话根因}
- **修复**：{改动概要，文件/分支/MR}
- **验证**：{测试结果/发布状态}

## 关键文件（断点恢复入口）
- 排查记录: {incidents 路径}
- Spec: {specs 路径}
- API: `.claude/api-knowledge/api-index.md`
- DB: `.claude/db-knowledge/db-tables-index.md`
- 工具: {相关脚本}

## 待办（后续工作）
1. {待办1}
2. {待办2}
3. {待办3}

## 当前问题（如有）
[粘贴压缩后想继续的具体问题]
```

## 填充规范

| 字段 | 内容 |
|------|------|
| 任务名 | 本次会话主任务（如 TICKET-001 物料匹配修复） |
| 根因 | 一句话结论（从 memory/incidents 提炼） |
| 关键文件 | 优先 incidents/README.md + specs/README + api/db 索引 |
| 待办 | 未完成的后续步骤（MR 合入/存量数据/联调等） |

## 关联

- [[session-compression-guide]]（压缩机制/断点续传）
- [[incident-reply-preferences]]（排查沉淀偏好）
- `research/上下文过长弊端与大模型影响.md`（为什么及时压缩）
