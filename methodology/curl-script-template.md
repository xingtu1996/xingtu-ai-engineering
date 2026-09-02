---
name: 方法-curl脚本模板
description: curl 接口验证脚本模板——一行 curl=接口探针，可脚本化回归
---

# 方法-curl脚本模板（curl Verification Script）

> 定义：**curl 接口验证脚本（curl Verification Script）** = curl 发 HTTP + `-w` 状态码 + jq 断言响应体 + SQL 核落库 + 幂等可重跑。

## 一、深入浅出
**本质**：一行 curl = 接口探针，把"发请求→验响应→核落库"固化成可重复回归的脚本。
**反模式**：① 只看 200 不验响应体→结构漂移静默；② 硬编码 token→泄露+过期即坏；③ 不幂等→重跑脏数据。

## 二、curl 骨架模板
```bash
TOKEN=${TOKEN:-}      # 环境变量注入，禁硬编码
BODY='{"skuCodes":["SKU001"],"activityId":"ACT-1","channelId":"CH-T"}'
code=$(curl -s -o /tmp/r.json -w '%{http_code}' -X POST \
  http://localhost:8111/api/promotion/stock/query \
  -H 'Content-Type: application/json' -H "Authorization: Bearer $TOKEN" -d "$BODY")
[[ $code == 200 ]] || { echo "FAIL $code"; cat /tmp/r.json; exit 1; }
jq -e '.stockList[0].skuCode=="SKU001"' /tmp/r.json >/dev/null || { echo FAIL-body; exit 1; }
mysql -h127.0.0.1 -N -e "SELECT COUNT(*) FROM t WHERE activity_id='ACT-1';" \
  | grep -q '^1$' || { echo FAIL-db; exit 1; }
echo PASS
```
**幂等**：mutating 先 SQL 预检 count=0 → 跑后断言 count=1 → 重跑安全。

## 三、常见模式表
| 模式 | 要点 |
|------|------|
| GET 查询 | URL 参数 + jq 断言字段 |
| POST 提交 | -d JSON + 200/400 + 落库核对 |
| 带 token 鉴权 | ${TOKEN:-} 注入，禁硬编码 |
| 文件上传 | -F "file=@path" |
| 分页 | 断言 total + 行数≤pageSize |
| 批量 | for 循环 + 单条失败即记 |

## 四、示例企业实践
| 实践 | 落地 |
|------|------|
| validator curl | specs-rules.md §5.4：AC 派生 TC-API-01…，L2≥3 条 |
| 本地栈 | dc-goods 8111 / dc-giveaway 18083（local-test-stack.md，RED-8） |
| 契约对照 | 请求体按 contracts/samples（dc-promotion-stock.query.ok.json）构造 |

## 五、检查清单
```
□ 状态码？200 + 4xx/5xx 全覆盖？
□ 响应体断言？jq 关键字段，非只 -w 看 200？
□ 幂等？mutating 有 SQL 预检+断言，重跑安全？
□ 落库核对？key 写操作 SQL 验证落地？
□ token 非硬编码？${TOKEN:-} 环境变量注入？
```

## 六、关联
`方法-api-testing-template.md` ｜ `specs-rules.md` §5.4 ｜ `contracts/samples/` ｜ `local-test-stack.md`
