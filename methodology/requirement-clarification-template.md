---
name: 方法-需求澄清模板
description: 需求澄清（Requirements Clarification / Grill）——模糊需求→精确约束，写码前一次问清省返工
---

# 方法-需求澄清（Requirements Clarification / Grill）

> 定义：**需求澄清（Requirements Clarification / Grill）** = 写码前把"模糊需求"烤成"精确约束"——敲定目标/AC/边界/依赖，减少 AI 猜测空间，一次问清省返工。
> 引用备案：GIGO 反面（`specs/从提示词到高质量Spec-驾驭方法论` 五要素）；feature-dev Step1；specs-rules §十一。

## 一、深入浅出

**一句话本质**：模糊需求→精确约束，减少 AI 猜测空间，一次问清省返工。

**反模式**：① 不懂就问被跳→AI 猜意图返工；② 带病开工（模糊不清零）→ 多轮修正+膨胀。

## 二、澄清追问清单骨架

逐项追问，答不了标"待确认"；**清零才允许开工**（constitution 铁律 1）。

| # | 维度 | 追问点 |
|---|------|--------|
| 1 | 业务目标 | 解决什么问题？不做会怎样？ |
| 2 | 用户故事 | 作为XX，我想XX，以便XX |
| 3 | 验收标准 | 怎么算做完？AC 可测？ |
| 4 | 边界 | 范围/排除项？与他需求冲突？ |
| 5 | 依赖 | 涉及服务？下游 Feign/MQ？ |
| 6 | 数据 | 表/字段变更？旧数据兼容？容量？ |
| 7 | MQ | 消息体变更？消费者幂等/重试？ |
| 8 | 安全三条件 | 计费/库存/权限：退出/幂等/回滚？ |

## 三、示例企业实践对应

| 方法论 | 示例企业落地 | 位置 |
|--------|---------|------|
| 澄清时机 | 方向未定先 EnterPlanMode 澄清+人审 → 批准后 Spec 固化 | `constitution.md` §五 |
| 全流程 | feature-dev 7 步（Step1：来源/故事/服务/依赖/限制） | `skills/feature-dev` |
| 追问方法 | grill-requirements：决策标 **DECIDED/RABBIT_HOLE/NO-GO**，模糊清零才编码 | `specs-rules.md` §十一 |
| 输入五要素 | 锚点/To-Be/澄清/坐标/约定 | `specs/从提示词到高质量Spec` |
| 影响面 | impact-analysis：Feign/MQ 消费者 + 共享表 3 跳 | `skills/impact-analysis` |

## 四、检查清单

```
□ 模糊地带清零？（无"待确认"→ 才开工）
□ AC 可测？（每条标 curl/SQL，P0/P1）
□ 影响面评估过？（impact-analysis + MQ 消费者）
□ 安全三条件？（退出/幂等/回滚）
□ 决策已标注？（DECIDED/RABBIT_HOLE/NO-GO）
□ 五要素齐？（锚点/To-Be/澄清/坐标/约定）
```
