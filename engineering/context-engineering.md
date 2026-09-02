---
name: 工程实践-上下文工程
description: 上下文工程实践模板——上下文窗口/token 的规划、压缩、预算管理，长会话不退化、短会话不浪费
metadata:
  type: project
---

# 工程实践-上下文工程（Context Engineering）

> 定义：对上下文窗口/token 的规划、压缩、预算管理——短会话不浪费、长会话不退化。
> 示例企业依据：token 六层优化（上行 spec→CBM→rtk→Headroom→模型；下行 Caveman+Concise→Ponytail）+ harness 体积标准（5% 规则、每 KB 常驻=上下文税）。

## 理念与权威引用

> 英文术语确认：**Context Engineering**（上下文工程），非 "Content Engineering"（内容工程，属内容营销领域）。业界共识正从 "Prompt Engineering" 演进到 "Context Engineering"：优化对象从单条 prompt 变为模型推理时看到的**完整上下文**（指令/知识/工具/记忆/状态/请求），本模板即其工程实践落地。

| 来源 | 作者/机构 | 年份 | 一句话核心理念 | 与本模板对应 |
|------|-----------|:---:|---------------|-------------|
| [A Survey of Context Engineering for Large Language Models (arXiv:2507.13334)](https://arxiv.org/abs/2507.13334) | Mei et al.（综述） | 2025 | Context Engineering 是超越 prompt 设计的正式学科：把"推理时喂给模型的完整信息负载"当作受 token 窗口/检索/记忆约束的优化问题，拆解为 instructions/knowledge/tools/memory/state/query 六组件 | 本模板"上下文预算分配（常驻 vs 按需）"即约束下的上下文组装优化 |
| [Effective Context Engineering for AI Agents（Anthropic 官方工程博客）](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) | Anthropic | 2025 | 目标是"找到最小的高信号 token 集合最大化期望输出"；上下文是稀缺资源，token 越多模型检索/推理能力越退化（context rot） | 本模板 5% 规则、每 KB 常驻=上下文税、渐进式披露 = Anthropic 的 just-in-time 加载 + 最小完整信息集 |
| [AI Engineering Playbook：How Should I Manage Context and Memory?](https://github.com/louisfb01/ai-engineering-cheatsheets) | Louis-François Bouchard（Towards AI 联合创始人/CTO） | 2026 | 上下文/记忆管理是 AI 工程师核心技能：compaction、滑动窗口、外部记忆、分叉会话等模式，防上下文溢出与长会话退化 | 本模板 /compact + 继续会话提示词 + STATUS 落盘 = compaction + 外部记忆模式 |
| [Agentic Context Engineering：Evolving Contexts for Self-Improving Language Models (arXiv:2510.04618)](https://arxiv.org/abs/2510.04618) | Zhang & Hu | 2025 | 把上下文当作可演化的 "playbook"，经 generator/reflector/curator 角色持续自我改进，无需微调 | 本模板"压缩后立刻输出继续会话提示词"即让上下文跨会话演化、越用越强 |

> 补充备案（待验证）：第三方报道称 OpenAI 2026-02 提出 "Harness Engineering" 概念，定位为 Context Engineering 之上的系统级约束/自动化验证/编排——与本项目 harness 体系命名巧合，无官方一手出处，仅备案不引证。

## 一、实践方法模板

**1. 上下文预算分配（常驻 vs 按需）**
- 常驻（CLAUDE.md + rules + memory）：只留"删掉会犯错"的，越大越退化。
- 按需/渐进式披露：CBM 查代码、skill 触发加载、incidents 查找——能按需就不常驻。

**2. 压缩触发**
- context ≥ 70% 主动提醒压缩。
- 时机：任务段落完成、关键决策落档后。
- 压缩后立刻输出「继续会话提示词」。

**3. 断点续传（继续会话提示词）**
- 模板：`templates/继续会话prompt-template.md`。
- 字段：任务背景 / 已完成（勿重做）/ 关键文件（断点入口）/ 待办 / 当前问题。

**4. 体积门禁**
- 改 CLAUDE.md / rules / hooks 后跑 `bash .claude/scripts/harness-size.sh -g`；FAIL = 治理信号（登记 CHANGELOG + 达标路径），不改口径消 FAIL。

## 二、骨架表

| 场景 | 手段 | 产出 |
|------|------|------|
| 检索代码省 token | CBM → ast-grep → grep 落点（2-3 服务） | 定位结论，非文件 dump |
| Bash 输出压缩 | rtk（hook 自动重写） | 输出减 60-90% |
| 回复输出压缩 | Caveman 散文压缩 | 回复 token 降 ~65% |
| 最少代码 | Ponytail YAGNI | 最小 diff |
| 长会话退化 | /compact + 继续会话提示词 | 上下文恢复续接 |
| harness 膨胀 | harness-size.sh -g | 体积门禁报告 |

## 三、约束/铁律

- **TOK-001~007**：CBM 优先 / 读源码用 snippet / Bash 加 rtk / 输出精简 / 最少代码 / CBM 索引按需 / Spec 精简。
- **5% 规则**：常驻 token ≤ 有效窗口 5%。
- **每 KB 常驻 = 固定上下文税**：>64K token 性能退化 30%+。
- **压缩前确认 STATUS 完整**：关键决策全落档，不靠对话记忆。

## 四、关联

- [[session-context-management]] / [[session-compression-guide]]（长会话五要素/压缩机制）
- `rules/token-optimization-rules.md` + `rules/harness-size-standard.md`（规范源，不复制全文）
