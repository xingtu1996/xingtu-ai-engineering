# 方法模板：DSL（Domain-Specific Language）— 把规则写成机器拦得住的形式

## 一、定义与权威引用
**定义**：DSL（Domain-Specific Language，领域特定语言）——面向特定领域、表达能力受限、可机器解析/校验的计算机语言。

**权威引用**：
1. Fowler《Domain-Specific Languages》(2010)：DSL =「面向特定领域的有限表达力语言」，分 internal/external。→ martinfowler.com/books/dsl.html
2. Fowler bliki DSL Q&A：「limited expressiveness focused on a particular domain」；价值 = 领域专家可读。→ martinfowler.com/bliki/DslQandA.html
3. Fowler 反过度设计(InfoQ 2007)：头号威胁 = 膨胀走向通用性（overgeneralization）。→ infoq.com/news/2007/11/martin-fowler-dsl-book/

## 二、深入浅出
**一句话本质**：把规则写成机器拦得住的形式，不只靠人看——SDD 靠纪律「看得见」，DSL 靠机器「拦得住」（漂移即红）。
**反模式**：为每场景造语言 = overgeneralization。**只对需机器校验的边界**（跨服务契约/P0 验收/状态机）DSL 化，克制勿全量化；内部逻辑用通用语言 + 测试覆盖。

## 三、示例企业实践对应
- **DSL 硬化**（CLAUDE.md 测试金字塔）：契约 JSON Schema 自动校验 = L2 强制层（漂移即红）；AC Gherkin 映射 = L1。人看门禁点，测试看其余。
- **contracts/ 落地**（引用不复制）：`.claude/contracts/` 存 JSON Schema（`additionalProperties:false` 防漂移）+ samples ok/bad；`validate-contracts.sh --all` 零依赖校验。真实案例：`dc-refund.gifts-materials-inbound`（TICKET-001 实测，`odoId=null`=匹配失败）。变更流程见 §四.4。
- 规范源：`specs-rules.md` §5.4 ｜ `contracts/README.md`（引用路径，不复制全文）。

## 四、骨架：DSL 化决策 + 契约 Schema + 验证
1. **场景判断**（三条全中才做）：需机器校验的边界｜结构可声明化（字段/枚举/状态）｜校验能自动跑（CI/hook）。反例：内部逻辑/一次性脚本。
2. **契约 Schema**：`schema/{名}.schema.json`（draft-07：`required`+类型+`enum`+`additionalProperties:false`）+ samples ok/bad 各一。
3. **验证**：`bash .claude/scripts/validate-contracts.sh --all` — 漂移即红，全绿才提交。
4. **变更流程**：改 schema+samples → 重跑 → 破坏性变更标注 diff → 追加 `contract-change-log.md`。

**版本历史**：2026-08-25 新建
