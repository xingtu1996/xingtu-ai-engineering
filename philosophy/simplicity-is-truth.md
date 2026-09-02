# 理念：简单即真（Simplicity / YAGNI）

## 定义
极简 = 高效。能删则删，已有的就是对的（RED-5 追加式）。只解今天的问题，不为假想的未来抽象。简单 ≠ 简陋：简单 = 充分理解。

> **引用备案**（权威来源，非闭门造车）
> - Agile 原则 10：*"Simplicity—the art of maximizing the amount of work not done—is essential."*（最大化"未做的工作"才是本质）— [agilemanifesto.org](http://agilemanifesto.org)
> - Kent Beck（YAGNI 出处）：*"Always implement things when you actually need them, never when you just foresee that you need them."* — [Fowler bliki: Yagni](https://www.martinfowler.com/bliki/Yagni.html)
> - Unix 哲学（ESR 规则 8）：*"Design for simplicity; add complexity only where you must."* — [The Art of Unix Programming](https://linuxclass.heinz.cmu.edu/doc/The-Art-of-Unix-Programming.html)

## 深入浅出
**一句话本质**：极简 = 高效，能删则删。复杂不是聪明，是没想透。

**反模式**：
- **过度工程**：为"迟早要用"建抽象/接口/配置 → 预测常错，真到用时需求已变
- **为未来抽象**：单实现套接口、单产品套工厂 → 抽象等有第二个时再抽
- **写多当交付**：输出量 ≠ 价值。Derek Wade 类比 W=F·d——使劲推但问题没动，就没做功

## 示例企业实践对应
- **Ponytail 阶梯**：不需要 → 已有 → 标准库 → 平台 → 已装依赖 → 一行 → 最少代码（先理解问题再爬梯）
- **RED-5 追加式**：已有的就是对的，只做加法不改旧
- **写通过删除**：每条规则问"删掉会犯错吗？不会 → 删"，只留"会犯错"的
- **体积预算**：harness 大小 = 上下文税（每 KB 常驻 × 每会话 = 固定成本），瘦身是复利
- **单一事实源**：知识只记一遍，防双写漂移

## 检查清单
```
□ 能删则删？  这行/这条删掉会犯错吗？不会 → 删
□ 已有复用？  仓库已有实现？标准库/平台覆盖？（Ponytail Ladder 2-4）
□ 新增必要？  "今天需要"还是"将来需要"？（YAGNI，将来需要 ≠ 现在做）
□ 最短可用？  一行能解决就一行，最短可工作的 diff 才是终点
```

> 完整论述见 `.claude/rules/harness-philosophy.md`（元哲学② + 原则10）+ `harness-size-standard.md`（预算）。
