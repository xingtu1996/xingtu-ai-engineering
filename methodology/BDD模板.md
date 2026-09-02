# BDD 方法论模板（Behavior-Driven Development）

> 定义：行为驱动开发（Behavior-Driven Development）——用**业务语言**描述系统行为，行为描述即可执行的验收测试。需求/测试/代码共用同一份规格。
> 用途：Spec `requirements.md` AC 派生测试、E2E 用例编写、Story 验收。**克制**：只对 P0 核心验收/状态/契约 DSL 化，勿全量化。

## 权威引用备案

| 来源 | 要点 |
|------|------|
| Dan North《Introducing BDD》(2006) | BDD 起源；GWT 源自《What's in a Story?》，称其"最有力的行为转变" |
| Martin Fowler bliki《GivenWhenThen》 | GWT 三句式由 Daniel Terhorst-North 与 Chris Matts 提出 |
| Cucumber 官方 Gherkin 参考（cucumber.io/docs/gherkin/reference） | Gherkin 语法标准；场景短小（3-5 步）；用声明式业务语言非技术步骤 |

## 一句话本质

让业务、测试、开发**用同一语言说话**——一份行为规格，同是需求文档、测试用例、验收标准。

## 反模式

| 症状 | 后果 |
|------|------|
| 需求写业务、测试写步骤 | 需求与测试脱节，验收靠人肉对 |
| 场景过细（点按钮/填字段） | 测试绑定 UI，改界面即碎 |
| 每条 AC 都上 Gherkin 全量化 | 文档膨胀，维护成本 > 收益 |
| Then 断言数据库内部字段 | 测试耦合实现，丢失业务语言 |

## 示例企业实践对应

- **Gherkin 克制映射**：`specs-rules.md` §5.4——AC 涉及 P0 核心验收时用 Given/When/Then 子集映射测试，克制勿全量化（SDD 靠纪律，DSL 靠机器，只对需机器校验的边界 DSL 化）
- **E2E 用例业务化**：`e2e-testing-rules.md`——用例命名带编号+场景中文（TC--AC10.1-前端已赠禁用），UI 操作 + 落库双重验证，截图即证据
- **Story GWT 验收**：`templates/需求交付Story模板.md` §4——作为/想要/以便 + 场景「假如/当/那么」，验收后回填 Spec `requirements.md`

## 骨架

```
作为 {角色}，我想要 {目标}，以便 {价值}        ← 用户故事（Story 编号 = TICKET-XXX）
场景：{一个业务行为}
  假如 {前置条件}（Given）
  当   {触发动作}（When）
  那么 {可观测结果}（Then）                    ← 只断言用户/系统可观测结果，不断言 DB 内部
```

**映射测试链路**：场景 → `requirements.md` AC → `validator.md` curl/SQL 断言（L2）→ E2E 用例（L3）。
