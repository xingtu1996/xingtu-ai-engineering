---
name: 方法-SDD模板
description: SDD 方法论模板——文档先行，Spec 唯一真相源
---

# 方法-SDD（Spec-Driven Development）

> 定义：**Spec 驱动开发（Spec-Driven Development）** = 文档先行：先写清"做什么/为什么/怎么算做完"，Spec 是唯一真相源，冲突时错的一定是代码。
> 引用备案：Meyer《Object-Oriented Software Construction》(1988) Design by Contract——先契约后实现；arXiv《From Code to Contract in the Age of AI Coding Assistants》(2026)——spec 生成代码零缝隙；Allegro《SDD best practices》(2026)——"the spec is the prompt"。

## 一、深入浅出

**一句话本质**：先写清"做什么、为什么、怎么证明做对"，再写代码——Spec 是给人和 AI 的超级提示词。

**反模式**：① 无文档直接写（vibe coding）→ 意图漂移返工；② 文档后补/跳过 → 与代码漂移、验收主观；③ 上下文遗忘 → 早期决策被忽略、越改越偏。

## 二、示例企业实践对应（引用不复制）

| SDD 方法论 | 示例企业落地 | 位置 |
|-----------|---------|------|
| 三条铁律（先 Spec/真相源/先改文档） | 铁律 1/2/3 | `constitution.md` §一 |
| 五件套 | analysis→requirements→design→tasks→validator | `specs-rules.md` §一 |
| 文档 + 执行双保障 | Spec 静态真相源 + Plan Mode | `constitution.md` §五 |
| 验证机器化 | validator curl/SQL + DSL 硬化 | `specs-rules.md` §五 |

### No Spec, No Code 门禁细则（constitution 铁律 1 + specs-rules 铁律 1）

> **铁律 1：No Spec, No Code — 没有文档，不准写代码。** 违反后果：方案未文档化→返工→时间浪费。

| 任务类型 | 文档门禁（至少完成数） | 来源 |
|---------|----------------------|------|
| 新功能 | `analysis` + `requirements` + `design` 至少 **2 份** | constitution §一 |
| Bug 修复 | `analysis`（根因 + 方案） | constitution §一 |
| 配置调整 | `requirements`（AC + 影响） | constitution §一 |
| 完整 Spec | analysis→requirements→design→tasks **四份 ≥2 份**才能写代码 | specs-rules §八 铁律 1 |

**Plan Mode 前置**：执行阶段启动 Plan Mode 前必须先有 Spec（需求澄清阶段 Plan 前置不受此限）——constitution §五。
**铁律 3 Reverse Sync**：发现 Bug 先改文档（analysis→requirements→validator）再改代码。

## 三、骨架：SDD 工作流

```
需求（用户故事+AC）→ 模糊先 EnterPlanMode 澄清
① analysis.md     现状/根因/方案对比/影响面（改什么/为什么）
② requirements.md AC 清单 + 影响范围（怎么算做完）
③ design.md       架构/接口/数据流 + 测试场景（怎么实现）
④ tasks.md        TDD 任务 + 依赖图 + Phase 状态（怎么推进）
⑤ validator.md    curl + SQL 验证（怎么证明）
→ 代码 → 审查 → 提交
```

**节奏**：每阶段产物 → 人审 checkpoint → 下一步；跳阶段 = 反模式；需求变更先改 Spec（铁律 3）再改码。

## 四、适用边界

- **用**：功能开发、跨服务改造、数据/MQ/Feign 变更、计费/库存敏感操作
- **不用**：单文件 <20 行 Bug、纯配置、原型（轻量只留 analysis+validator，specs-rules §二）

## 五、关联

- `rules/constitution.md` §一 + §五 ｜ `rules/specs-rules.md` ｜ `templates/parallel-spec-dev-sop.md`
