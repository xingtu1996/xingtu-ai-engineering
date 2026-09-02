# 理念：驾驭分工（Judgment over Execution / Human-in-the-loop）

## 定义
AI 产证据链与方案，**人做判断与方向**。工具会换，思想不换——判断权归人（部署方），执行权授权给 AI，各守其位。

> **引用备案**（权威来源，非闭门造车）
> - Anthropic《Trustworthy agents in practice》：人类控制为核心原则；Plan Mode 把人工提升到审整体方案，执行中仍可介入 — [anthropic.com/research/trustworthy-agents](https://www.anthropic.com/research/trustworthy-agents)
> - Anthropic《How we contain Claude across products》：HITL 沙箱把"人可干预"落到基础设施层 — [anthropic.com/engineering/how-we-contain-claude](https://www.anthropic.com/engineering/how-we-contain-claude)
> - 《What Humans Should Approve Is Intent, Not the Diff》：人审的是意图/方案，不是机器产物的 diff；仅不可逆动作升级回人 — [dev.to](https://dev.to/shimo4228/what-humans-should-approve-is-intent-not-the-diff-a-decision-table-for-agent-approval-gates-1a3j)

## 深入浅出
**一句话本质**：AI 是手，人是脑——AI 产证据链与方案，人做决策与方向。

**反模式**：
- **AI 自说自话**：AI 自己定方案、自己评审、自己放行 → 判断权旁落，无人把关
- **人做执行活**：人盯代码细节/逐动作审批 → 浪费判断力，方向层反而失守
- **判断权外包**："什么叫完成/风险多大"交给模型默认 → 方向错了，执行再快也白费

## 示例企业实践对应
- **HITL 交互确认**：关键节点回人确认，不单方面下结论（`memory/interaction-confirm-mode.md`）
- **驾驭工程模板**：AI 产证据链与方案，人做判断与方向 → `templates/工程实践-harness-engineering.md`
- **双保障机制**：Spec 静态真相源 + Plan Mode 动态执行上下文，人审 Spec/AC 批准后执行
- **约束墙**：质量靠测试金字塔/validator/契约机器拦，人只看关键路径与门禁点

## 检查清单
```
□ 关键节点回人？  方案审批/合并/发布是否回人？（编码 AI 自主，终审人拍板）
□ AI 产证据链？    结论有代码/SQL/日志三证据，还是 AI 空推断？
□ 人确认方向？    "什么叫完成/风险可接受"由人定，非模型默认？
□ 判断权未旁落？  出现 AI 自定方案+自评+自放行闭环？
```

> 完整论述见 `.claude/rules/harness-philosophy.md`（元哲学⑤ + 概念"驾驭分工"）。
