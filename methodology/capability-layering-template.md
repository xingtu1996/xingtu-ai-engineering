# 方法：能力分层（Capability Tiers / Maturity Levels）

> 定义：人的能力决定**用哪层工具 + harness 介入多少**。从「开发者熟练度 / 工具使用级 / AI 自主度 / 组织成熟度」四维定位人和团队现在在哪、下一步升哪——**能力越强 → 工具层越高 → AI 自主度越高 → 组织成熟度越高**。
> 引用备案：全景 §三/§六/§七（`research/2026082514-harness-panorama-methodology/00_README.md`）｜Cutler 5 阶段成熟度模型（cutler.sg）｜DORA 2024｜Gartner 2026｜方法-工具分层 L0~L4

## 深入浅出

**一句话本质**：能力定工具层级与 harness 介入度——越强上越高层工具、给 AI 越多自主；新手靠 harness 兜底（介入度高），专家被 harness 放行（介入度低，甚至自建）。

**反模式**：

| 反模式 | 后果 | 正解 |
|--------|------|------|
| 新手硬上高层工具（L3/L4） | 门槛高不会用、输出不可控 | 低层起步 + 强 harness 兜底（模板/规则全量） |
| 专家被低层限制（L0/L1） | 不可控、没效率 | 上高层工具，减 harness 束缚 |
| 全员套同一层（不按能力分层） | 新手够不着 / 专家被拖累 | 四维定位，按人分层 |

## 四维能力层级表

**① 开发者熟练度**（Development Proficiency）

| 层级 | 可用工具层 | harness 介入度 |
|------|:---:|------|
| 新手 | L0/L1 | 高：模板/规则全量兜底，先套骨架 |
| 中级 | L2 | 中：核心 rules + 触发加载 |
| 高级 | L3 | 低：按需，重审查 |
| 专家 | L4 | 自建：把 harness 做成产物 |

**② 工具使用级别**（Tool Usage Tier，映射方法-工具分层 L0~L4）

| 层 | 工具 | 能力要求 | 交互范式 |
|----|------|------|---------|
| L0 裸聊 chatbot | ChatGPT 网页 | 无 | 纯对话 |
| L1 外行低代码 | Lovable/Bolt/n8n | 非程序员 | 描述式生成 |
| L2 中间 IDE | Cursor/Copilot | 中高级开发者 | 协作式 |
| L3 专业 CLI | Claude Code/Codex | 专业开发/架构师 | 命令式执行 |
| L4 OS harness | Omarchy/dsh | 平台团队自建 | 环境式，Agent 一等公民 |

**③ AI 自主度**（AI Autonomy）

| 层级 | 特征 | 人保留 |
|------|------|:---:|
| 人全干 | AI 0%，纯手写 | 100% |
| AI 辅助 | AI 补全/建议，人逐行审 | ~70% |
| AI 驱动 | AI 主导执行，人审关键决策点 | ~30% |
| AI 主导 | AI 自主执行，人只做判断/决策 | **10%**（判断做什么/文件删否/模型选型/客户意图） |

**④ 组织成熟度**（Org Maturity，Cutler 5 阶段）

| 阶段 | 名称 | 特征 |
|:---:|------|------|
| 1 | Individual | 个人试用，无共享 |
| 2 | **Reusable** | 有可复用资产——**多数团队卡死的 plateau**（DORA：AI 采用 +25% 但交付稳定性 -7.2%，正是卡 Stage 2 信号） |
| 3 | Enforced | 强制执行（hook/门禁机械拦截，规则变墙） |
| 4 | Delegated | 委派（Agent 独立执行，人审决策） |
| 5 | Distributed | 组织级分布式（Gartner 2026：80% 大型工程组织设专职平台团队） |

## 四层联动关系

```
能力↑ → 工具层↑ → AI 自主度↑ → 组织成熟度↑
（开发熟练度）（L0→L4）（人全干→AI主导）（Stage 1→5）
```

- 佐证（全景 §六/§七）：专业者用低层工具不可控、外行用高层工具不会用 → **匹配 > 求高**；人保留 10% 判断边界（判断该做什么/文件删否/模型选型/客户意图）；瓶颈从生成转向验证，审查角色权重上升。
- 卡点识别：卡工具层 = 能力没跟上；卡 Stage 2 = 只沉淀未强制（缺 Enforced 机械层）。
- 示例企业落点：单人 AI 选手 = 演进线中间态偏左；多 Agent 蜂群侦查（12 investigator + 3 adversarial）+ multi-agent-orchestration 形态选择 = Stage 3→4 的 Delegated 特征。

## 骨架（定位 → 提升 → 积木包）

```
① 定位当前层：四维各答一问
   熟练度？工具层？自主度？组织 Stage？
② 下一步提升路径（只升一维，不贪多）
   新手→中级：套模板 + L1→L2
   中级→高级：学审查 + L2→L3
   高级→专家：自建 harness + L3→L4
   Stage 2→3：把规则做成 hook 机械强制
③ 对应积木包（示例企业）
   skills/（场景流程）→ rules/（护栏）→ hooks/（机械墙）→ agents/（角色）→ workflows/（SOP）
```

## 案例

- **新手反例**：给业务同学直接上 Claude Code CLI（L3）→ 不会用、输出不可控 → 正解：先 L1 低代码/L2 IDE + 强模板兜底。
- **专家反例**：资深工程师只用 ChatGPT 网页（L0）逐段贴代码 → 不可控低效 → 正解：上 L3 CLI + 自建 harness。
- **组织卡 Stage 2**：团队沉淀了 rules/skills 但只靠人自觉执行 → DORA 稳定性下滑 → 正解：把高频红线做成 hook/门禁机械拦截（Enforced），再谈委派。

## 检查清单

- □ 已四维定位当前层？（熟练度/工具/自主度/组织 Stage）
- □ 工具层匹配能力？（新手不上 L3/L4，专家不用 L0/L1）
- □ 卡在哪层？下一步只升一维？有对应积木包？
- □ Stage 2 卡死信号？（只沉淀未强制 → 补 hook/门禁）

## 引用备案（背书双轨）

**外部权威**：

| 权威来源 | 作者/年 | URL | 要点 |
|---------|--------|-----|------|
| Capability Maturity Model（CMM） | Carnegie Mellon SEI 1991（CMM v1.0） | https://www.cmmiinstitute.com/（CMMI 继承） | 能力成熟度 5 级（Initial→Repeatable→Defined→Managed→Optimizing），每级是下级的基座、跳级几乎必然返工——"能力定层级 + 只升一维"的原始出处 |
| 《5-Stage Maturity Model for AI-Augmented Engineering Teams》 | Cutler 2026-05 | https://cutler.sg/blog/2026-05-five-stage-maturity-model-ai-engineering-teams | Individual→Reusable→Enforced→Delegated→Distributed；Stage 2（Reusable）是多数团队卡死的 plateau——本文 ④ 组织成熟度直接引用 |
| DORA 2024 / Gartner 2026 | Google Cloud DORA / Gartner | https://dora.dev/ / https://www.gartner.com/ | AI 采用 +25% 但交付稳定性 -7.2%（卡 Stage 2 信号）；2026 年 80% 大型工程组织设专职平台团队（Stage 5 佐证） |

**内部实证（示例企业落地）**：

- `.claude/research/2026082514-harness-panorama-methodology/00_README.md` §三/§六/§七 — 方法论全景：Cutler/DORA/Gartner 佐证原文（§三）+ D12 条目 URL 归档（§六），是本文四维层级的源头与"匹配 > 求高"依据
- 示例企业落点（本文 §四层联动）：单人 AI 选手 = 中间态偏左；12 investigator + 3 adversarial 蜂群 + multi-agent-orchestration 形态选择 = Stage 3→4 Delegated 特征
- 工具使用级 L0~L4 对应 `.claude/toolbox/`（工具箱全集）+ `方法-工具分层模板.md`（L0 裸聊→L4 OS harness 的现实映射）

## 版本历史

| 日期 | 版本 | 变更 | 变更人 |
|------|:---:|------|--------|
| 2026-08-26 | V1.0 | 新建：能力分层四维（熟练度/工具级/自主度/组织成熟度） | Claude Code（操盘：XingTu） |
| 2026-08-26 | V1.1 | 体积预算放宽为软约束：补回佐证（DORA/Gartner）、交互范式列、案例、检查清单 | Claude Code（操盘：XingTu） |
