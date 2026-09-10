---
name: 理念-复利进化
description: 复利进化哲学实践模板——使用→问题→沉淀→反哺→下次更好，越用越强
---

# 理念-复利进化（Compounding Evolution）

> **定义**：harness 越用越强的自进化循环——使用发现的问题沉淀、反哺成下次自动拦截的机制，复利式成长。错只犯一次。
> **依据**：`rules/harness-philosophy.md` 元哲学③ + 原则 5~8（引用不复制）。

## 〇、引用备案

| 引用 | 来源 | 对应 |
|------|------|------|
| Argyris & Schön 双环学习：单环修症状，双环改底层假设 | Wiley《Revitalizing double-loop learning》 | 沉淀是改规则/Gate 拦一类错，非修一次 Bug |
| ASI-Evolve：knowledge→hypothesis→experiment→analysis 闭环重复，越转越强 | arXiv 2507.21046《A Survey of Self-Evolving Agents》 | 复利 loop 的 AI 侧验证 |
| 自指改进无外部落地 → "compounding entropy"；须人留环内 | ACM《Is Recursive Self-Improvement Really Here?》 | 反模式锚点：沉淀必须落机制，不靠记忆 |

## 一、深入浅出

**本质**：使用→问题→沉淀→反哺→下次更好，越用越强。

**反模式**：
- 错了不记：排查完不落 incidents/BCI = 白干，同类错重踩。
- 产出无落点：功能/调研/复盘不登记 = 黑盒，harness 不涨。
- 只记不改机制：只写文档不提炼红线/Gate = 下次仍靠记忆（靠不住）。

## 二、示例企业实践对应（引用不复制）

| 落点 | 位置 |
|------|------|
| 复利 loop 骨架 | `templates/工程实践-loop-engineering.md` |
| 坑→BCI→Gate | `memory/bad-case-index.md`（BCI-001~029） |
| 生产问题 | `incidents/{TICKET-XXXX}-{简述}-{日期}/`，一工单一目录 |
| 调研/选型 | `research/`（多源+溯源） |
| 规则/红线反哺 | `rules/constitution.md` + 场景 rules |

## 三、检查清单（产出后自查）

```
□ 坑有 BCI？——根因→现象→预防 Gate
□ 发现反哺 harness？——落到 incidents/BCI/红线/Gate/CHANGELOG，还是留在会话
□ 单一事实源？——先查后写，知识只记一遍
□ 有地方落实？——零黑盒，非纸面
□ 反哺只做加法？——RED-5 不覆盖已有规则
```

## 四、关联

`harness-philosophy.md` ｜ `工程实践-loop-engineering.md`（操作落地）｜ `bad-case-index.md` ｜ `specs-rules.md` §十一
