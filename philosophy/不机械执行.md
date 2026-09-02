---
name: 理念-不机械执行
description: 不机械执行实践模板（Intent over Compliance / Anti-Mechanical Execution）
---

# 理念-不机械执行（Intent over Compliance / Anti-Mechanical Execution）

> 定义：**清单是候选不是命令，先懂为什么再执行**。
> 示例企业依据：harness 哲学 原则 2/11，规范源 `rules/harness-philosophy.md`，只引用不复制。

## 〇、引用备案

| # | 权威来源 | 要点 | 对应落地 |
|---|---------|------|---------|
| 1 | A. Fortuna《process addiction》'24 | "checkbox compliance"——团队"go through the motions"不知为何 | 照单全抄=合规表演 |
| 2 | Catapult Labs《Breathing Life...》 | 仪式变"checklist items, stripped of intended purpose" | 清单剥掉"为什么"即失效 |
| 3 | E. Kim《Just Follow the Protocol》(Milgram/阿伦特) | "knowledge of purpose...not mindless obedience" | 纪律性遵循≠盲从，判断权不可让渡 |

> 术语边界：取 Intent over Compliance，区别于 Compliance Gap。

## 一、深入浅出

**本质一句话**：清单是候选不是命令，先懂为什么再执行。

**反模式（机械执行）**：
- 照单全抄：逐项勾掉却不解意图——"我不懂含义，但我勾了"。
- 为合规而合规：以"走了流程"免责，产出对错没人管。
- 清单当护身符：勾满即安心，不质疑清单本身是否过时。

## 二、示例企业实践对应

| 实践 | 落地位置 | 作用 |
|------|---------|------|
| 意图路由 | 工作流"动词+目标"意图识别 | 先懂意图再定路由 |
| 测试金字塔分工 | 人看关键路径和门禁点，测试看其余 | 判断力投关键处 |
| 清单理解意图 | delivery-checklist / gap-analysis / BCI | 先问"为什么查"再落证据 |
| BDD 意图路由 | AI 分析意图+建议深度，人确认后执行 | 人保留判断权 |

## 三、检查清单

```
□ 理解意图了吗？  这条为什么存在？不查会出什么错？
□ 清单过对抗+人审？被质疑过还是顺手抄的？
□ 机械执行了吗？  在"过场"还是真验证了产出？
□ 裁剪有依据吗？  跳过/新增有理由？
□ 结果有人复核？  勾了≠对，证据链留档了吗？
```

## 四、关联

- `rules/harness-philosophy.md`（原则 2 不机械执行 / 11 机械生效，规范源）
- CLAUDE.md 测试金字塔（人看关键路径和门禁点）
- `memory/bad-case-index.md`（勾过≠做对的历史教训）
