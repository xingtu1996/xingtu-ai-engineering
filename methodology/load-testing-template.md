---
name: 方法-压测模板
description: 压测模板（Performance/Load Testing），stages/perf
---

# 方法-压测模板（Performance/Load Testing）

> 定义：**压测/性能测试（Performance/Load Testing）** = 用模拟流量量化系统容量、延迟、稳定性。

## 一、深入浅出

**一句话本质**：不是测"对不对"，是测"扛不扛得住、快不快"——TPS、响应时间、稳定性闭环。

**反模式**：① 只在生产出问题才压测；② 数据不真实（假数据/无 think time）；③ 只看平均不看 P95。

## 二、压测类型表

| 类型 | 英文 | 目的 | 加载 |
|------|------|------|------|
| 基准 | Benchmark | 单用户基线 | 1 并发跑通 |
| 负载 | Load | 验证容量达标 | 升到目标 TPS |
| 压力 | Stress | 找瓶颈/崩溃点 | 超容量加压 |
| 峰值 | Spike | 突增扛得住 | 骤升骤降 |
| 稳定性 | Soak | 长跑泄漏/退化 | 满负荷 24-72h |

## 三、核心指标

| 指标 | 说明 | 参考阈值 |
|------|------|---------|
| TPS/QPS | 吞吐量 | 匹配流量模型 |
| 响应时间 | P50/P95/P99，禁只看平均 | P95 ≤ 1s |
| 错误率 | 失败占比 | ≤ 1% |
| 资源 | CPU/内存/连接池/GC | ≤ 80% |

## 四、压测工具

| 工具 | 适用 |
|------|------|
| JMeter | 复杂场景/分布式（GUI 慎用） |
| ab | 快速单接口 |
| wrk | 高并发 HTTP，易测 P95/P99 |
| k6 | CI/CD 压测 + 基线对比 |

## 五、示例企业实践

| 落点 | 内容 | 位置 |
|------|------|------|
| stages/perf | ⚪ 远期，复用 stress-utils-stresstester | `test/README.md` |
| 容量预警 | 寿命/降级标注 + 80%/90% 两级预警 | `capacity-design-rules.md` |
| 性能回归 | 冒烟压测入 CI，p95 超阈值即红 | 同 + `e2e-testing-rules.md` |

## 六、骨架：一次压测六步

```
场景设计（流量模型+目标 TPS/P95）→ 数据准备（真实量级防缓存失真）
→ 压测执行（非 GUI，预热+阶梯加压）→ 指标采集（TPS/P95/错误率/资源）
→ 瓶颈定位（慢 SQL/连接池/GC/线程池）→ 容量结论（最大 TPS+假设标注）
```

## 七、检查清单

- [ ] P95/P99 达标（对照均值，尾部未掩盖）
- [ ] 错误率 ≤ 1%｜资源 ≤ 80%
- [ ] 容量假设已标注（寿命+预警，capacity-design-rules §二）
- [ ] 数据真实（环境/流量/量级贴合生产）
- [ ] soak 无泄漏（GC 后内存回落）

## 八、关联

`test/README.md`｜`capacity-design-rules.md`｜`方法-quality-gate-template.md`｜`e2e-testing-rules.md`

## 九、引用备案（背书双轨）

**外部权威**：

| 权威来源 | 作者/机构 | 年份 | URL | 一句话理念 |
|---------|---------|:---:|------|-----------|
| Apache JMeter 官方用户手册 | Apache 软件基金会 | 维护中 | https://jmeter.apache.org/usermanual/index.html | 压测事实标准工具：CLI 非 GUI 模式 + 分布式压测，多协议（HTTP/JDBC/JMS）；对应本模板"压测工具"节 JMeter 行 |
| JMeter 官方 Best Practices | Apache 软件基金会 | 维护中 | https://jmeter.apache.org/usermanual/best-practices.html | 压测最佳实践：真实数据防失真、非 GUI 执行、结果分析；对应骨架六步"数据准备/非 GUI/指标采集" |

**内部实证**：

| 案例 | 位置 | 证明什么 |
|------|------|---------|
| 压测阶段目录已预留 | `test/stages/perf/`（⚪ 远期，`test/README.md` L20 标注 stresstester/验收/巡检） | 示例企业已为压测划定落点，非纸面方法论 |
| 容量预警规范 | `rules/capacity-design-rules.md`（设计寿命标注 + 80%/90% 两级预警） | 压测结论必须标注容量假设的机制保障（对应检查清单"容量假设已标注"） |
| UID 容量溢出事故 | `incidents/UID时间位数溢出-20260721/`（28bit 时间因子仅覆盖 8.5 年，2026 年溢出） | 真实容量假设失败的活案例：不做压测/容量边界的代价 |
