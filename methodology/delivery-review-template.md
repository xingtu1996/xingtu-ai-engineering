# 方法-交付检视模板（Delivery Checklist / Gap Analysis）

> 宣称"代码完成"前的最后一道闸——六维 26 项交付检视 + 全链路差距分析，防"假完成"。触发于 Phase 5 验证前 / MR 前。
> 引用（不复制）：`memory/delivery-checklist.md` + `memory/gap-analysis-checklist.md`。体积：约 2.1KB。

## 定义

代码写完 ≠ 功能完整。交付检视 = 五维逐项勾选；差距分析 = 沿「前端→后端→导出→导入→MQ 消费者→SQL」找缺口。两者配合，堵住"只测自己链路"的假完成。

## 深入浅出

**本质**：宣称"代码完成"前，过六维 26 项 + 全链路差距分析（含 MQ 消费者），否则"完成"只是你一段，非用户。

**反模式**：只测自己链路（Controller 通就说完成）｜漏 MQ 消费者（批导事件无 `@StreamListener`）｜不查导出/导入（DTO 缺失页面没反应）｜只看编译不看回归（已有功能坏了不知）。

## 六维 26 项骨架（引用 delivery-checklist，完整逐项见源）

| 维度 | 关键项 | 教训 |
|------|--------|:---:|
| 代码 | Controller 覆盖前端全部 API；新建类先搜已有；Feign 注册 @ComponentScan；版本与基线一致；枚举 @JsonCreator | 重复 Controller |
| SQL | DDL/DML 入 `sql/`；SIT+UAT 执行；字段 3 层一致（SQL/Excel/DTO）；已有数据不受影响；可重复执行 | 漏执行/字段不一致 |
| 前端 | Tab/路由注册；API 路径一致；导入导出 ServiceType 与 SQL 一致；导出 URL 一致；已有功能回归 | Tab/导出全缺 |
| 配置 | bootstrap-local.yml；config.yaml；SIT/UAT/PROD 环境差异 | 本地起不来 |
| 测试 | 编译零错误；本地 curl 全覆盖；导出可下载；DB 前后一致；已有功能回归；进程清理（RED-6） | 假完成 |

## 6 步差距分析（引用 gap-analysis-checklist）

```
1. 读前端代码 → 收集所有 API 调用
2. grep Controller 确认 API 存在
3. grep Service 确认业务逻辑存在
4. grep DTO 确认导入导出存在
5. grep @StreamListener 确认 MQ 消费者存在（批导必查！）
6. 读 SQL 目录确认脚本匹配
```

## 检查清单

- [ ] 六维 26 项逐项勾选
- [ ] 6 步差距分析跑完
- [ ] MQ 消费者链路已查（批导必查）
- [ ] 导出/导入链路已查（DTO/ServiceType）
- [ ] 未部署 SIT 也敢断言"代码完成"

## 版本历史

| 日期 | 版本 | 变更 | 变更人 |
|------|:---:|------|--------|
| 2026-08-25 | V1.0 | 新建 | Claude Code（操盘：XingTu） |
