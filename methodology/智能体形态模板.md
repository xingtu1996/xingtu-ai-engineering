# 方法-智能体形态模板（Agent / Subagent / Workflow Patterns）

> 定义：多 Agent 任务按复杂度选形态：串行/Subagent 并行/Workflow 编排。
> 引用：Anthropic《Building Effective Agents》(2024-12, 8 编排模式) +《Common workflow patterns》(2025)。
> 落点：`skills/multi-agent-orchestration/`（决策）+ `templates/agent-template.md`（Agent）+ `workflows/_TEMPLATE.md`（Workflow）。

## 一、本质

Agent=独立执行单元，Subagent=主 Agent 派生的执行子单元，Workflow=程序化编排容器（执行单元=Subagent）。形态按复杂度选，够用即止。
**反模式**：单任务套重 Workflow｜多任务全串行｜不按依赖并行（共享文件冲突）｜N=1 开 Subagent。

## 二、三形态选型表

| 维度 | Agent | Subagent | Workflow |
|------|-------|----------|----------|
| 定义 | 独立执行单元 | 主 Agent 派生子单元 | agent/pipeline/parallel 编排容器 |
| 上下文 | 自持载入 | 派生即隔离 | 逐 agent 隔离 |
| 适用 | 单域专家/investigator 蜂群 | 2-5 文件 1-2 服务并行 | 跨 3+ 服务/需拓扑序屏障 |
| 示例企业落点 | agent-template.md | Agent 工具派生 | workflows/_TEMPLATE.md |

## 三、8 模式 → 示例企业形态

| # | 模式 | 示例企业形态 |
|---|------|---------|
| 1 | Prompt Chaining | pipeline()/Spec 阶段 |
| 2 | Routing | 意图路由/蜂群路由 |
| 3 | Parallelization-Sectioning | parallel() 按服务拆 |
| 4 | Parallelization-Voting | adversarial 三角色投票 |
| 5 | Orchestrator-Workers | 主 Agent+investigator 蜂群 |
| 6 | Evaluator-Optimizer | adversarial-optimizer/budget 循环 |
| 7 | Sequential Workflow | 单 Agent 串行 |
| 8 | Autonomous Agent | 成本 10-100×，谨慎用 |

## 四、骨架入口

- 新 Agent → 复制 `templates/agent-template.md` 改 frontmatter，放 `agents/{name}.md` 即注册
- 新 Workflow → 复制 `workflows/_TEMPLATE.md` 改 meta+编排，存 `workflows/{name}.js`
- 多任务/多 Spec → 加载 `multi-agent-orchestration` skill（DAG+优先级+契约+文件冲突）选形态

## 五、检查清单

```
□ 选对形态？（N=1→串行；2-5 文件→Subagent；跨 3+ 服务→Workflow；P0→串行）
□ 依赖分析过？（CBM 画 DAG；接口/数据依赖→先契约再并行）
□ 文件冲突验证？（修改清单交集=∅）
□ 上下文隔离？（只读自己 Spec+contracts）
□ 成本可控？（并发 ≤16 建议 3-5；无并行价值不强制）
```

## 引用备案

### 外部权威（方法论可信度背书）

| 来源 | 作者 | 年份 | URL | 背书要点 |
|------|------|:---:|------|---------|
| 《Building Effective Agents》 | Anthropic Engineering | 2024-12 | https://www.anthropic.com/engineering/building-effective-agents | workflows（预定义路径）vs agents（动态自驱）区分 + 8 编排模式（Prompt Chaining/Routing/Parallelization/Orchestrator-Workers/Evaluator-Optimizer 等）→ 本模板"三、8 模式 → 示例企业形态"表出处 |
| 《Common workflow patterns for AI agents》 | Anthropic Engineering | 2025 | https://claude.com/blog/common-workflow-patterns-for-ai-agents-and-when-to-use-them | 三常用模式（prompt chaining/routing/parallelization）适用判断 + "先最简单，观察再升级" → 对应"够用即止/无并行价值不强制" |

### 内部实证（示例企业真实用过）

| 落点 | 类型 | 链接 | 实证内容 |
|------|------|------|---------|
| `skills/multi-agent-orchestration/SKILL.md` | Skill | `.claude/skills/multi-agent-orchestration/SKILL.md` | 多 Spec/多任务形态决策落点（依赖分析 + 优先级分级 + 形态选择） |
| `templates/agent-template.md` | 样板 | `.claude/templates/agent-template.md` | Agent frontmatter 注册样板（新 Agent 复制改 frontmatter） |
| `workflows/_TEMPLATE.md` | 样板 | `.claude/workflows/_TEMPLATE.md` | Workflow 编排骨架（meta + 编排，agent()/pipeline()/parallel()/phase()） |
| `workflows/07-multi-session-parallel-exec.md` | 工作流 | `.claude/workflows/07-multi-session-parallel-exec.md` | 多会话并行执行实证（Subagent/Workflow 实际调度案例） |
