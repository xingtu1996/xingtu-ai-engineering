# 工程实践-Feign 异常处理模式（示例企业）

> 用途：跨服务 Feign 调用失败的异常处理统一风格，避免"裸 500 误报障"踩坑（TICKET-002 实证 2026-08-27）
> 来源：dc-order GuestGiftApplication 修复（500 误报障）+ CustomErrorDecoder 发现

## 〇、核心教训（为什么需要此模板）

TICKET-002 客需随礼超量提交返回裸 500（无 err_msg），前端误报障。排查发现：
- dc-order 用**自定义 `CustomErrorDecoder`**：Feign 非 2xx → `new RuntimeException(response body JSON)`，**不是标准 FeignException**
- 按 FeignException 假设写的 catch 没覆盖 → 裸 500

**教训**：写 Feign 异常处理前，**先查项目 Feign 异常机制**（ErrorDecoder / fallback / 拦截器），再设计处理。

## 一、Feign 异常处理设计模式（示例企业统一风格）

### 1. 先查项目 Feign 机制（必做）
```
grep -rln "ErrorDecoder\|CustomErrorDecoder\|FallbackFactory" <服务>/src/main/java
```
| 机制 | 异常形态 |
|------|---------|
| 自定义 CustomErrorDecoder | `RuntimeException(message=响应body JSON)` |
| 标准 Feign | `FeignException`（contentUTF8() 取 body）|
| Hystrix fallback | `HystrixRuntimeException`（cause 链含 FeignException）|

### 2. 统一异常解包 + err_msg 提取（模板代码）
```java
catch (Exception e) {
    String errMsg = extractErrMsg(orderId, e);  // 多源提取
    log.error("Xxx调用失败, orderId={}, errMsg={}", orderId, errMsg, e);
    throw new BusinessException(errMsg);         // 转业务异常（BaseExceptionTranslator 处理）
}

private String extractErrMsg(String bizKey, Exception e) {
    String msg = parseErrMsg(e.getMessage());              // ① CustomErrorDecoder: message=body JSON
    if (msg != null) return msg;
    if (e instanceof FeignException) {                     // ② 标准 Feign
        msg = parseErrMsg(((FeignException) e).contentUTF8());
        if (msg != null) return msg;
    }
    FeignException feignEx = unwrapFeignException(e);      // ③ Hystrix 包装
    if (feignEx != null) {
        msg = parseErrMsg(feignEx.contentUTF8());
        if (msg != null) return msg;
    }
    return "服务调用失败或不可用";
}

private String parseErrMsg(String json) {
    if (json == null || json.isEmpty()) return null;
    try {
        String m = JSON.parseObject(json).getString("err_msg");
        return (m != null && !m.isEmpty()) ? m : null;
    } catch (Exception ex) { return null; }
}
```

### 3. 单测覆盖（必须）
- CustomErrorDecoder RuntimeException（message=body JSON）
- 标准 FeignException（contentUTF8）
- Hystrix 包装（unwrap）
- 非 JSON → 兜底

## 二、异常出库排查/论证方法论（三层归因 + 本地复现）

| 步骤 | 方法 | 证据 |
|------|------|------|
| 1 部署确认 | Rancher Pod 镜像 / actuator build-info | dc-order 1.6.30.0 + commit |
| 2 日志归因 | SLS 查异常（先定 logstore：fs-uat-applog） | CustomErrorDecoder RuntimeException 堆栈 |
| 3 数据核对 | db-query 落库（订单行/库存/幂等） | 无孤儿行 |
| 4 本地复现 | 本地启动（关 Eureka + Feign 直连 UAT）→ 复现 | 日志异常解包 |
| 5 根因判定 | 代码 + 数据 + 日志三证据 | CustomErrorDecoder 未覆盖 |

## 三、关键文件/关联
- 排查模板：`production-troubleshooting-template.md`
- 设计模式参考：项目 `FeignSupportConfig$CustomErrorDecoder` / `GravityCommonInterceptor`
- 案例：spec TICKET-002 `26_库存链路E2E测试方案` / `GuestGiftApplication`
