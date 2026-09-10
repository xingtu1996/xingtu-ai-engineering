---
name: 理念-真相第一
description: 真相第一理念实践模板——实测>推断、诚实>好看，结论需证据、承诺落机械验证
---

# 理念-真相第一（Truth First / Evidence-based）

> 定义：**真相第一 = 实测 > 推断，诚实 > 好看，Spec is Truth。** 结论必须有证据，承诺必须落机械验证（hook/脚本/门禁）。
> 示例企业依据：`rules/harness-philosophy.md` 元哲学① + 原则 1/3/11/12。

## 〇、权威引用备案

| # | 权威引用 | 来源 | 对应 |
|---|---------|------|------|
| 1 | Anthropic 评价驱动开发（EDD）：Evals 是 AI 的"单元测试"，实现前定期望、开发中盯回归 | claude.com/blog/improving-skill-creator | 先 Spec 后 Code + 测试金字塔 |
| 2 | Basili 等：测量难不是靠直觉的借口，"别像巫医，要像科学家" | cs.umd.edu/~basili/publications/technical/T110.pdf | 排查用三证据链 |
| 3 | Okareo：Evals 是 Agentic AI 的 CI/CD 活契约，回归即安全网 | okareo.com/blog/evals-are-ci-for-agentics | 机械门禁（size.sh/契约漂移即红） |

## 一、深入浅出

- **本质**：实测 > 推断——机器跑出来的才算数；诚实 > 好看——错的数据比没有更危险。
- **反模式**：臆测代替验证（没跑说"应该没问题"）；推断冒充实测；口径漂移（门禁 FAIL 靠改口径消红）；只报喜不报忧（撤回夸大比维护假数字更保值）。

## 二、示例企业实践对应

- **harness 哲学**：元哲学① + 原则 1 不闭门造车 / 3 第一性原理 / 11 机械生效 / 12 约束墙。
- **实证案例**：HARNESS-GOV 对抗审计撤回"8.5x 夸大"——数据误读（SUM 汇总行）撑出的漂亮指标，按第一性原理核实后撤回。好看≠实测，就是负债。
- **机械门禁**：文字声明 ≠ 事实。`harness-size.sh -g` 防回弹、`validate-contracts.sh` 漂移即红、L1≥85% / L2 curl≥3。

## 三、检查清单

```
□ 结论有实测证据吗？（代码/SQL/日志）
□ 诚实口径吗？（撤回夸大比维护假数字更保值？）
□ 机械验证了吗？（承诺落到 hook/脚本/门禁，还是只靠嘴说？）
□ 区分已验证/待验证/推测了吗？（不把推断写成结论）
□ 数字有来源吗？（无来源标"待查"）
```

## 四、关联

- `rules/harness-philosophy.md`（规范源）
- `rules/harness-size-standard.md` §五（诚实口径）
- `rules/harness-gate.md`（8.5x 夸大教训）
