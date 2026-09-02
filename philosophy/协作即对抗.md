---
name: 理念-协作即对抗
description: 协作即对抗实践模板（Adversarial Collaboration）
---

# 理念-协作即对抗（Adversarial Collaboration）

> 定义：**单一视角有盲区，靠对抗 + 可观测补齐**。持不同立场的双方为同一目标协作、互为审查，而非各自为战。
> 示例企业依据：harness 哲学 ④ + 原则 4/13，规范源 `rules/harness-philosophy.md`，只引用不复制。

## 〇、引用备案

| # | 权威来源 | 要点 | 对应落地 |
|---|---------|------|---------|
| 1 | Kahneman《Adversarial Collaboration》EDGE 演讲 | 心理学起源：对立学派同做实验共出结果，接受平局 | 三角色独立出结论后收敛 |
| 2 | Nature《Make science more collegial》 | 以合作取代互撕，提速科学辩论 | 产出解决点非驳倒点：先共识后分歧 |
| 3 | GOAT（ICML 2025）自动化红队 | 独立攻击方找漏洞，ASR@10 达 91-96% | 独立视角可机械化：自动扫描+契约交叉验证 |

> 术语边界：取 Adversarial Collaboration，区别于纯 Red Teaming（进攻式测试）。

## 一、深入浅出

**本质一句话**：单一视角必有盲区，靠对抗找盲区、靠可观测证清白。

**反模式（自说自话）**：一人既写码又拍板（自我确认偏差）；审查只说"看起来对"不跑数据；结论无代码/SQL/日志证据；改动无提交链接/diff，无人能复核。

## 二、示例企业实践对应

| 实践 | 落地位置 | 作用 |
|------|---------|------|
| 三角色对抗审查 | `skills/adversarial-review/`（refuter/optimizer/auditor）| 多视角独立审查，非一人既运动员又裁判 |
| 白盒可观测 | `memory/whitebox-commit-visibility.md`（提交链接+diff+责任到人）| 改动可见、可被复核 |
| 交叉验证 | 证据链三证（代码+SQL+日志）｜三段式检索（CBM+ast-grep+grep）｜validator/契约 DSL | 结论靠机器拦，非口头声明 |

## 三、检查清单

```
□ 多视角独立审查？ ≥2 角色独立出结论再交叉
□ 数据实测？         结论有代码/SQL/日志证据
□ 结果可观测？       提交链接/diff 留档、责任到人
□ 公平刻画反方？     不歪曲对方立场
□ 收敛非互撕？       产共识+分歧，接受平局
```

## 四、关联

- `rules/harness-philosophy.md`（④协作即对抗，规范源）
- `skills/adversarial-review/`（三角色对抗审查）
- `memory/whitebox-commit-visibility.md`（白盒可观测）
