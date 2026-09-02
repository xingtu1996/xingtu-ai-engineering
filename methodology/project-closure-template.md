---
name: 方法-项目结项模板
description: 业务侧项目结项交付——交付证据+复盘沉淀+知识归档，供业务/相关方验收收尾
---

# 方法-项目结项（Project Closure / Retrospective）

> 定义：**项目结项（Project Closure / Retrospective）** = 交付证据+复盘沉淀+知识归档的结构化总结。
> 引用备案：某零售电商项目 `deliverables/` 交付物 + `templates/session-review-template.md`。

## 一、深入浅出

**一句话本质**：结项 = 交付证据 + 复盘沉淀 + 知识归档——不是"做完了"一句话，是给业务一个可验收、可追溯、可复用的收尾。

**反模式**：① 只报喜无量化（业务无法核验）；② 只交付不沉淀（下项目从零检索）；③ 遗留责任悬空（收尾即烂尾）。

## 二、结项报告骨架（八段）

```
① 项目概览  名称/周期/目标/范围/角色/Ticket
② 交付清单  交付物+验收人+状态
③ 验收证据  AC 逐条+验证方式+链接
④ 量化成果  工期/行数/覆盖率/缺陷率（数字带来源）
⑤ 复盘教训  做对/踩坑→BCI/规则
⑥ 资产沉淀  research/specs/memory/incidents 归档位置
⑦ 风险遗留  未决项+责任人+跟进计划（不掩盖）
⑧ 责任交接  各模块负责人+交接物+后续入口
```

## 三、示例企业实践对应

| 方法论 | 示例企业落地 | 位置 |
|--------|---------|------|
| 项目结项复盘 | 某零售电商项目溯源论证+四方对抗审查（边界不掩盖） | `specs/<项目>/deliverables/` |
| 会话复盘模板 | T6 小颗粒复盘，结项时聚合为项目级 | `templates/session-review-template.md` |
| 量化数据实证 | 52 天 1 人+AI Agent 交付约 16 万行（900+ 文件/50 提交） | `research/<日期>-harness-panorama/00_README.md` |
| 沉淀到 research/specs/memory | 调研→research/，执行→specs/，教训→memory/BCI | `research/README.md`｜`specs/README.md` |

## 四、检查清单

```
□ 交付证据?（清单+验收人+AC 逐条，非口头"做完了"）
□ 量化?（工期/行数/覆盖率，有来源可核验）
□ 复盘教训?（做对/踩坑+提炼 BCI，非只报喜）
□ 资产归档?（research/specs/memory 落盘可检索）
□ 遗留责任?（风险/待办有责任人，无悬空）
```

## 五、关联

- `session-review-template.md`｜`mr-template.md`/`e2e-test-report-template.md`（验收证据）｜`specs-rules.md`（Spec ✅ 收尾）
