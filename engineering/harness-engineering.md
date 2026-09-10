---
name: 工程实践-驾驭工程（Harness Engineering）
description: 驾驭工程实践模板——人机分工、Spec 驱动。AI 产证据链与方案，人做判断与方向
metadata:
  type: project
---

# 工程实践-驾驭工程（Harness Engineering）

> 定义：驾驭工程 = 人机分工。**AI 产证据链与方案，人做判断与方向**。
> 示例企业依据：Spec 驱动 + Spec+Plan 双保障（`rules/constitution.md` §一/§五）+ 驾驭方法论（`specs/从提示词到高质量Spec-驾驭方法论_20260803.md`）。

## 〇、理念与权威引用（英文术语 + 权威来源）

> **英文术语确认：Harness Engineering（驾驭工程）** — Anthropic 官方「harness」系列文章确立的学科名：构建围绕模型的执行框架（工具/流程/检查点/人机分工），让 AI 可靠地产出证据链与方案、人负责判断与方向。这是「人机分工 + AI 驾驭 + Spec 驱动」最贴合的英文对应。
> 其中「人做判断与方向」对应 Anthropic 的**判断工程化（Engineering for Judgment）**理念：把主观质量量化为可打分标准、执行者不给自己打分（独立评估者）、人在最薄弱环节把守终审。

| # | 权威来源 | 核心观点 | 与本模板的对应 |
|---|---------|---------|---------------|
| 1 | Anthropic《Building Effective Agents》(2024-12) | Workflow=预定义路径编排；Agent=模型动态自控；evaluator-optimizer 让生成与评判分离；agent 必备 human checkpoints；从简单开始，评估证明不足再加复杂度 | Spec 驱动=预定义路径（workflow）；对抗审查/validator=evaluator-optimizer；HITL 关键节点回人=human checkpoints |
| 2 | Anthropic《Effective Harnesses for Long-Running Agents》(2025-11) | 长任务两败因：一次做太多+过早宣布胜利；解法=初始 agent 把需求展开为 feature_list.json（默认 FAIL）+增量提交+progress 交接 | 铁律 1 No Spec No Code=默认 FAIL；Spec 固化=需求展开为可测清单；回写 memory/incidents=progress 交接 |
| 3 | Anthropic《Harness Design for Long-Running Application Development》(2026-03, Anthropic Labs) | GAN 启发拆分 doer 与 critic：Planner/Generator/Evaluator 三 Agent；sprint contract 先约定「什么叫完成」；把设计质量量化为可打分标准；评估者用全新上下文、无写权限 | 澄清→Spec→执行→审查闭环=Planner→Generator→Evaluator；对抗审查三角色=独立 Evaluator；AI 产证据链、人终审=判断工程化 |
| 4 | ICML 2026《Judgment Operators》论文 | 决策时拦截 agent 动作：Allow/Edit/Escalate/Deny 四语义；Escalate 保留人工审查路径；「判断应被部署方拥有并审计」 | RED-5/禁止模式清单=拦截语义；方案审批/合并/发布必回人=Escalate；人做判断与方向=判断归属部署方 |

> **同构佐证**：Cheesecake Labs《Harness Engineering: Why "Done" Isn't the Agent Saying So》——「完成」不是 agent 说了算，而是系统证明出来的（与本模板铁律 2 Spec is Truth 同构）。
> **一句话对应**：驾驭工程=人机分工的工程化落地——AI 是执行与证据（Generator），Spec 是握手协议（Planner 产物），对抗审查/validator 是独立评判（Evaluator），人把控方向与终审（Engineering for Judgment）。

## 一、驾驭流程模板（需求 → 闭环）

```
需求 → 澄清（EnterPlanMode：追问歧义 + 方案对比 + 人审）
     → Spec 固化（analysis→requirements→design→tasks→validator）
     → 人审（审 Spec/AC，批准后执行）
     → 执行（Plan 动态层：读 Spec → 设计 → 编译 → validator 验证）
     → 回写（更新 Spec 日志 + tasks 状态 + memory/incidents）
```

> 执行前必须先有 Spec（铁律 1）；Plan 中发现 Spec 遗漏 → 先改 Spec 再继续（铁律 3）。

## 二、骨架表：环节 → AI 职责 → 人职责 → 产出

| 环节 | AI 职责 | 人职责 | 产出 |
|------|---------|--------|------|
| 澄清 | 追问模糊点、列方案对比、识别歧义 | 描述需求、澄清决策 | 需求结论 |
| 方案 | 技术方案 + 影响面 + 对比表 | 审批选型、拍板 | 获批方案 |
| 编码 | 按 Spec 执行 + TDD 自测 + 级联自查 | 门禁抽查 | 符合 Spec 的代码 |
| 审查 | 对抗审查 + 规范自检 + 交付清单 | 终审、合并、验收 | 通过门禁 |
| 提交 | 提交 + 回写 memory/incidents + 链接 | 确认发布 | 可追溯变更 |

## 三、约束/铁律

- **铁律 1 No Spec No Code**：文档未完成 → 禁止写代码（新功能至少 2 份，Bug 修复需根因+方案）。
- **铁律 2 Spec is Truth**：文档与代码冲突 → 错的一定是代码。
- **铁律 3 Reverse Sync**：发现 Bug → 先修文档补遗漏，最后改代码。
- **RED-5 追加式**：只做加法，不改已有枚举/方法签名/API 路径/DB 记录。
- **HITL 关键节点回人**：方案审批/合并/发布必回人；编码 AI 自主。

## 四、关联

- `rules/constitution.md` §一/§五（铁律+双保障）——规范源，不复制全文
- `specs/从提示词到高质量Spec-驾驭方法论_20260803.md`——输入 5 要素（锚点/To-Be/澄清/坐标/约定）
- `templates/parallel-spec-dev-sop.md`——多需求并行时人机分工规模化
