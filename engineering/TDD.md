---
name: 工程实践-TDD
description: TDD（Test-Driven Development）实践模板——先写失败测试→最小实现→重构，测试先行把需求变成可执行约束
---

# 工程实践-TDD

> 定义：**Test-Driven Development（TDD）** = 先写测试 → 实现 → 重构 的循环。核心是"测试先行"：需求先变成可执行约束（失败测试），再写让测试通过的最少代码。
> 权威引用备案：Kent Beck《Test-Driven Development: By Example》(2003) 两规则——「无失败测试不写一行代码」「消除重复」；TDD 本质是**设计技术**，测试是副产品。Mike Cohn《Succeeding with Agile》提出**测试金字塔**：底层单元测试占多数（快/便宜），顶层 E2E 最少（慢/脆）。

## 一、深入浅出

- **一句话本质**：测试先行 = 需求先变成可执行约束（🔴），再写代码让它通过（🟢）——代码由需求驱动，不由猜测驱动。
- **反模式**：写完代码补测试（测试变摆设、只刷覆盖率）；不写测试直接联调（问题后置到 SIT/生产才暴露）。
- **心智**：🔴 定义"做到什么算完成" → 🟢 只写通过所需的最少代码 → 🔵 安全网下清理结构。

## 二、示例企业实践对应

| 示例企业机制 | 对应 TDD 环节 | 落点 |
|---------|:---:|------|
| Constitution RED-8 | 本地测试 TDD 左移（能本地 curl 验证不等 SIT） | 编译→本地启动→curl→合并 |
| 测试金字塔 L1 | 单元测试 JUnit5，Service ≥85% | Service 方法级红绿循环 |
| specs-rules §5.5 | tasks.md Phase 2：a🔴→b🟢→c🔵 | tasks.md 状态跟踪 |

> 引用不复制，规范源见 `rules/constitution.md` §三 RED-8、`rules/specs-rules.md` §5.5、CLAUDE.md「测试金字塔」。

## 三、骨架

### TDD 循环（方法级循环）

```
🔴 Red     先写失败测试（断言行为，看它红 = 测试有效）
🟢 Green   最小实现让测试通过（fake it → 泛化，防过度设计）
🔵 Refactor 安全网下消除重复、整理结构，测试保持绿
```

### 测试金字塔（下多上少，Bug 越早越便宜拦）

```
   L3 E2E（curl 全链路，最少）
  L2 API 契约（validator curl ≥3 条）
 L1 单元（JUnit5，Service ≥85%）← TDD 主战场
```

## 四、关联

- `rules/constitution.md` §三 RED-8（本地测试 TDD 左移）
- `rules/specs-rules.md` §5.5（tasks.md 红绿重构循环）
- CLAUDE.md「测试金字塔」（6 层质量防线，L1 单元 Service ≥85%）
