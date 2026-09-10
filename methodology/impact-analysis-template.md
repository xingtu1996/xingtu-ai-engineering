---
name: 方法-影响分析模板
description: 影响分析（Impact Analysis）——改动前级联评估：改一个服务先查 3 跳下游，防基线 BUG
---

# 方法-影响分析（Impact Analysis）

> 定义：**影响分析（Impact Analysis）** = 修改一处代码前，评估对系统其余部分的连锁影响（下游、消费者、契约），识别风险面后改。
> 引用备案：软件维护经典实践（Sommerville）；对应 blast radius 爆炸半径。

## 一、深入浅出

**一句话本质**：改一个服务，116+ 微服务可能受影响——改动前 3 跳评估下游；不看下游直接改 = 埋基线 BUG。

**反模式**：① 只看直接调用→漏间接消费者；② 不看下游直接改 DTO/表结构/MQ 消息体→反序列化失败；③ 全仓 grep→应 CBM 精准确认。

## 二、示例企业实践对应（引用不复制）

| 方法论 | 示例企业落地 | 位置 |
|--------|---------|------|
| 3 跳级联影响 | CBM `trace_path` depth=3 | `CLAUDE.md` 零-B |
| 高警戒服务 | dc-order/promotion/giveaway 全量 grep | `constitution.md` §二 |
| 消费者追踪 | Feign 调用方 / MQ `@StreamListener` / 共享表读写方 | `impact-analysis/SKILL.md` |
| CBM 盲区补漏 | ast-grep 提取 `@FeignClient`/`@StreamListener`/`@Resource` | `CLAUDE.md` 零-C |

## 三、骨架：级联影响分析五步

```
① 改动点 变更文件+层（controller契约/application逻辑/domain表/client Feign/MQ listener）
② 3 跳影响面 CBM trace_path depth=3 → 受影响服务清单
③ 消费者清单 Feign 调用方/MQ 消费者/共享表读写方（ast-grep 补盲区）
④ 契约变更 DTO/消息体/路径 → 更新 contracts/ + validate-contracts.sh
⑤ 风险分级 P0 反序列化失败/库存错乱 ｜ P1 逻辑偏移 ｜ P2 边缘 → 建议测试范围
```

## 四、检查清单

- [ ] 3 跳影响面查了？
- [ ] 消费者全列？（Feign 调用方/MQ 消费者/共享表读写方）
- [ ] 事务内无远程调用？（`@Transactional` 内禁 Feign/RabbitMQ/HTTP）
- [ ] 契约变更登记？（contracts/ + validate）
- [ ] 高警戒服务全量 grep？
- [ ] 风险分级+回归范围？

## 五、适用边界

- **用**：改 Feign 接口/DTO、MQ 消息体、共享表、`@Transactional` 边界、库存 SQL、dc-order/promotion/giveaway
- **不用**：单文件 <20 行纯内部逻辑（无跨服务出口）

## 六、关联

- `skills/impact-analysis/SKILL.md` ｜ `CLAUDE.md` 零-B/零-C ｜ `constitution.md` §二/§六 ｜ `contracts/`
