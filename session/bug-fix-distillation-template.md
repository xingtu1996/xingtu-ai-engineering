# BUG 修复沉淀 — {问题名}

> 来源：项目沉淀模板（`.claude/incidents/_TEMPLATE/BUG修复沉淀-模板.md`）
> 用途：incidents 落盘用——每个生产问题修复后，用本模板提炼 BCI 条目，防同类问题复发。

## 从这次事故中学到什么

{3-5 条具体教训，每条一句话}

## BCI 条目提炼

> 关联：BCI 索引在 `.claude/memory/bad-case-index.md`，本模板只负责提炼流程骨架，条目规则以索引文件为准。

| 字段 | 值 |
|------|-----|
| ID | BCI-{NNN} |
| 技术栈 | {Spring Boot/MyBatis-Plus/React/...} |
| 失败症状 | {一句话，grep 可用，例：`grep -rn "xxx" dc-*/src/main/java/`} |
| 文件证据 | {出问题的代码模式，`grep 'xxx' dc-*/src/main/java/` 可搜索} |
| 修复模式 | {怎么修，`grep 'yyy' dc-*/src/main/java/` 可验证} |

## 同类问题排查命令

```bash
# 示例企业 Java 服务内搜索同类型问题
grep -rn "{关键词}" dc-*/src/main/java/
```

## 是否需要 Hard Gate

- [ ] 是 → 提炼为红线，新增到 `.claude/rules/constitution.md` 禁止模式清单
- [ ] 否 → 仅记录到 BCI，Code Review 时自动匹配
