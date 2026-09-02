# 并行 Spec 开发 SOP（契约优先 · 多模块）

> 来源：某项目 parallel-spec-sop（示例企业契约优先方案回流泛化版）
> 用途：跨服务/多模块需求突破「1 人 1 模块串行」瓶颈
> 体积：约 3KB（带案例略超 2KB 预算，SOP 类特批）

**核心**：写码前先把模块间握手协议定死（契约），锁定后无依赖模块完全并行。
理论/调度/门禁见 specs-rules §九/§十、CLAUDE.md「Subagent 并行调度规则」、constitution.md §七。

| 阶段 | 动作 | 产出 |
|:---:|------|------|
| 0 | 全局上下文收集 | 模块清单/流程图/ER 草图 |
| 1 | 依赖分析 | 依赖图 + 串/并行分组 |
| 2 | 契约定义 | `.claude/contracts/` 契约 |
| 3 | Spec 拆分 | N 个独立 Spec |
| 4 | 并行执行 | N 个独立会话 |
| 5 | 集成收口 | 契约验证 + 联调 |

**P0 全局收集**：AI 读全需求材料（PRD/原型）+ CBM → 模块清单/流程图/ER。检查点「要建什么？」
例：需求=订单确认后调支付建支付单；模块=Order/Payment/Inventory；流程=下单→扣库存→建支付单→确认

**P1 依赖分析**：CBM 检索+依赖推理 → 依赖图 → 分组。

| 依赖类型 | 策略 |
|---------|------|
| 接口依赖（A 调 B API） | 串行：先定契约→并行 |
| 数据依赖（共享表/字段） | 串行：先改 schema→并行 |
| 单向引用（A import B DTO） | 串行：先定 DTO→并行 |
| 完全独立 | **并行** |

例：Order→Payment/Inventory；Inventory∥Payment → 并行组[Inventory,Payment]，串行组[Order→*]

**P2 契约定义（关键）**：每跨模块点一份契约落 `.claude/contracts/`（JSON Schema+ok/bad samples，`validate-contracts.sh --all` 锁定）。必含 Provider 签名/Consumer 幂等/错误降级/测试用例。**人须签字**。
例：
```
OrderPayment: provider=PaymentService POST /api/payment/create
  req{orderId,amount} resp{paymentId,status} err{code,message}
  consumer=OrderService 时机:订单确认后 幂等:同orderId不重复 降级:3s重试1次→待支付
```

**P3 Spec 拆分**：每模块独立 Spec（`{YYYYMMDDHH}-{Ticket}-{domain}`）`00~05+constitution.md`。先根模块（不被依赖）后叶子模块，并行组同时分配。M 级以上先 adversarial-review。

**P4 并行执行**：每模块**独立会话**（非 workflow subagent）：上下文隔离/可 resume/review 清晰。**并行度≤3**。
```
Step1 主会话=指挥中心
Step2 独立会话：「读 Spec→守 contracts/+constitution→按 tasks 执行→跑 validator」
Step3 轮询审核：过→标记；不过→同会话续修
Step4 全完成→P5
```
门禁：编译零错/单测 PASS/validator 过/constitution 零违规/CBM 3 跳。

**P5 集成收口**：`validate-contracts.sh` 验契约；串行组按拓扑序合入，验握手点（幂等/超时/降级）。
例：两服务完成→联调 `POST /api/payment/create` 验幂等（同 orderId 两次→同 paymentId）与降级。
