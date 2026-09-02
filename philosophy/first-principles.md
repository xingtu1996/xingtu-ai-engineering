# 理念：第一性原理（First Principles Thinking）

## 定义
分解到不可再分的基本事实，再从基础重建——不靠类比、惯例或"别人都这么做"。源自古希腊亚里士多德，马斯克用于逆向工程式解复杂问题。

> **引用备案**（权威来源，非闭门造车）
> - 亚里士多德（源起）：第一性原理（archai）是事物最基本的出发点，其他一切由此推出 — [cio-wiki](https://cio-wiki.org/index.php?title=First_Principles_Thinking)
> - Elon Musk（2013 TED）：*"boil things down to the most fundamental truths and then reason up from there, as opposed to reasoning by analogy"*——类比只是"copying what others do with tiny variations"— [inc.com](https://www.inc.com/ayse-birsel/want-to-think-like-elon-musk-first-you-need-to-forget-what-you-think-you-know.html)
> - Farnam Street 三步框架：逆向工程复杂问题——①质疑假设 ②拆解基本要素 ③从事实重建新解 — [同上 cio-wiki]

## 深入浅出
**一句话本质**：分解到不可再分的基本事实，再从基础重建——而非从类比/惯例照抄。

**反模式**：
- **类比照搬**："别人都这么做/以前一直这样"当理由，不回到本质（= tiny variations）
- **惯例当真理**：把惯例/历史当物理定律——人类规则可质疑，物理定律不可改
- **假设未验证**：隐含假设没列出来、没核实就往下走

## 示例企业实践对应
- **实测 > 推断**：机器跑出来才算数，推理/直觉不冒充证据（原则 3）
- **实证案例**：harness-gate"空名 $23.9 异常"误判——AI 把 SUM 汇总行误读为真实异常，按第一性原理回数据源头核实后排除。像异常，拆到基本事实是正常汇总。
- **不闭门造车，引用不复制**：先搜 2026 成熟方案（WebSearch/research/incidents），引用带 URL；但引用≠照抄，落地按本地事实核实（借鉴思想不复制结论）

## 检查清单
```
□ 从基本事实出发？  拆到不可再分的事实，还是从类比/惯例推的？
□ 质疑隐含假设？    "应该这样"有依据吗？人类惯例还是物理定律？
□ 实测验证？        跑出来的还是推断的？数据源头核实了吗（空名教训）？
□ 引用不复制？      借鉴了权威来源，落地按本地事实核实了吗？
```

> 完整论述见 `rules/harness-philosophy.md`（原则 3）+ `harness-gate.md` §五（空名误判）。
