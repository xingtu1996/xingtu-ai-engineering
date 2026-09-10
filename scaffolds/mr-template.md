# MR 模板（示例企业 FS 适配）

> 来源：某零售集团 harness 套件 `templates/MR-template.md`
> 用途：MR 提交前自查清单，逐项勾选，未满足项说明理由或阻塞合并。
> 体积：约 2.3KB，门禁项多略超 2KB 预算（允许例外）。

## 提交信息
- Commit：`t-[TICKET]-[SUBTASK]-[简述]-[作者]`（`rules/git-rules.md`；严禁 Co-Authored-By / `--no-verify` / `--force`）

## 变更摘要
- 新增 / 修改 / 删除：[文件/类/方法]

## 关联 Spec
- `specs/{Ticket}-{domain}/`（无 spec 说明理由，见 `rules/constitution.md` 铁律 1）

## 测试
- [ ] 单元测试：Service 覆盖率 ≥85%（constitution §七 L1）
- [ ] 接口测试：validator curl ≥3 条（L2）
- [ ] 本地验证：`build.sh clean install -DskipTests` 零错误 + 启动通过
- [ ] 测试后进程已清理（RED-6）

## 数据库变更
- [ ] 升级脚本：`ALTER TABLE` 表名带库名（`rules/java-rules.md` §三）
- [ ] 回滚脚本：逆向 DDL 已准备
- [ ] 变更前 `git status` 确认目标已追踪（`rules/security-rules.md` §一）

## 配置变更
| 配置项 | 旧值 | 新值 | 说明 |
|--------|------|------|------|
| | | | |

## 评审检查清单
- [ ] 编码规范：DDD 分层正确、方法 ≤50 行、`#{}` 参数化（`rules/java-rules.md`）
- [ ] 级联影响：CBM 3 跳已查，**高警戒 dc-order / dc-promotion / dc-giveaway 全量 grep**（constitution §二）
- [ ] 事务内无远程调用（Feign/RabbitMQ）
- [ ] MQ 消费者幂等（`rules/mq-rules.md`）
- [ ] 外部调用 try-catch + log.error + 降级
- [ ] 追加式开发，无覆盖已有功能（RED-5）
- [ ] 注释只讲业务逻辑，无流程编号（`rules/java-rules.md` §五）

## 安全检查
- [ ] 无硬编码密钥/密码明文（`rules/security-rules.md`）
- [ ] SQL 全 `#{}` 参数化，无 `${}`
- [ ] 日志敏感信息脱敏（`139****1234`）
- [ ] Controller 层 `@Valid` 输入校验

## 风险与回滚
| 项 | 内容 |
|----|------|
| 风险等级 | 低 / 中 / 高 |
| 影响范围 | [服务/接口/数据] |
| 回滚方式 | 镜像回退 / db_rollback.sql / 配置回退 |
| 回滚脚本 | [路径或命令] |
| 回滚验证 | [验证步骤] |

## 部署说明
- 配置变更：无 / [说明] ｜ 依赖变更：无 / [说明] ｜ 部署顺序：无 / [说明]
