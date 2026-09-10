# 本地测试模板（Local Testing / TDD Left-shift）

> 定义：本地灰盒测试（Local Testing）——提交合并前，本地启动最小依赖栈（Eureka + 相关服务），curl 灰盒验证接口（200 + 预期字段 + 落库核对），通过再合并。TDD 左移在示例企业的落地。
> 用途：提交前自测 / Bug 修复验证 / 新接口联调。

## 引用备案

| 来源 | 要点 |
|------|------|
| Constitution RED-8 | TDD 左移：编译→本地启动→curl 验证→合并 |
| Constitution RED-6 | 测完清理进程：kill 残留 + `ps aux \| grep java` |
| memory/local-test-stack.md | 灰盒环境：Eureka 8001 / dc-goods 8111 / dc-giveaway 18083 |

## 一句话本质

能本地 curl 验证的不等部署——**本地通过再合并**：编译零错误 + 本地启动 + curl 全链路通过，才允许合入 SIT。

## 反模式

| 症状 | 后果 |
|------|------|
| 本地不测直接部署 | 问题后置 SIT/生产才暴露 |
| 测完不清理进程 | 内存泄露 + 端口占用，下次启动失败 |
| 裸 `mvn` 编译 | 缺内部依赖，编译失败 |
| Feign 直连改动不恢复 | 测试配置带进提交 |

## 示例企业实践

- **编译**：`bash .claude/scripts/build.sh clean install -DskipTests`（JDK8 + 内部 settings，禁裸 mvn）
- **本地栈**（local-test-stack.md）：Eureka 8001 → dc-goods 8111 → dc-giveaway 18083（追加 `allow-bean-definition-overriding=true`）
- **curl 全链路**：等 `curl /actuator/health` 200 → 调业务接口 → 200 + 预期字段
- **环境**：profile local（默认），中间件见 `.claude/config.yaml` §environments.local
- **Feign 直连**：已有接口临时解 url 直连 SIT/UAT，⚠️ 测完恢复不提交
- **进程清理（RED-6）**：测完立即 kill，`ps aux \| grep java` + `lsof -i :<port>` 确认

## 骨架

```
① 编译     build.sh clean install -DskipTests → 零错误
② 启动依赖  Eureka → dc-goods → dc-giveaway，逐一等 health 200
③ curl 验证 目标业务接口 → HTTP 200 + 预期字段断言
④ 数据核对  写操作必须落库验证
⑤ 清理     kill 全部 java + lsof 确认端口释放 + 恢复临时改动
```

## 检查清单

- [ ] 本地编译零错误（build.sh）
- [ ] curl 全链路验证过（200 + 预期字段）
- [ ] 写操作落库核对过
- [ ] 进程全部 kill（`ps aux | grep java` 无残留）
- [ ] 端口释放（`lsof -i :<port>` 无 java 占用）
- [ ] 临时改动已恢复（不提交）
