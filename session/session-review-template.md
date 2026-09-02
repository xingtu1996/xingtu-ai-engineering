# 会话复盘模板（T6 会话复盘/协作复盘）

> 用途：会话结束/压缩前落盘，记录已完成工作 + 沉淀资产 + 断点续传，供跨会话复用。
> 参考实例：`memory/session-record-20260811--guest-gift-e2e.md`、`session-record-20260812-p0-e2e.md` 等 7 份。
> 来源：2026082513-template-system-analysis/template-index.md T6。

---

## 会话复盘：{主题}（{日期} 会话）

### 一、会话背景
- 主题 / Ticket / 关联分支 / session 原始文件（`~/.claude/projects/{项目}/*.jsonl`，便于溯源）

### 二、本次完成（含提交/合并）

| 项 | 内容 | 提交/合并 |
|----|------|----------|

### 三、关键文件/资产
- 改动的代码路径 + 关键决策（一句话）

### 四、沉淀资产
- 写入 memory/incidents/rules/specs 的条目（链接）

### 五、待办/断点续传
- 下一步做什么（+ 继续会话提示词要点）

---

## 填写指引
- 提交 ID 必填（可追溯）；决策写"为什么"，不写流程编号
- 体积 <2KB；超长拆子文档
- 落盘位置：`memory/session-record-{YYYYMMDD}-{主题}.md`
