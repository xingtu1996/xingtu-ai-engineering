# 服务目录结构样板（新服务脚手架）

> **用途**：新服务脚手架，AI 有样学样生成目录+创建步骤。
> 基准：`CLAUDE.md`「Typical DC Service」+ `java-rules.md` §二/§三。

## 一、目录结构树

```
dc-<service>/
├── src/main/java/com/示例企业/ctf/<service>/
│   ├── controller/      # 收参→校验→调 application
│   ├── application/     # 业务编排+事务（核心层）
│   ├── domain/          # 领域对象+Mapper 接口
│   ├── client/          # Feign+fallback 降级
│   ├── query/           # CQRS 查询层
│   ├── listener/        # MQ 监听→委托 application
│   ├── infrastructure/  # DAO/Mapper 实现
│   └── utils/           # 工具类
├── src/main/resources/
│   ├── bootstrap.yml        # 默认 local
│   ├── bootstrap-dev.yml    # 开发
│   ├── bootstrap-qa.yml     # QA
│   └── bootstrap-remote.yml # 生产
└── pom.xml              # parent=ctf-parent/adpt-parent
```

## 二、DDD 分层职责（java-rules.md §二）

| 层 | 做什么 | 禁止 |
|----|--------|------|
| controller | 收参→校验→调 application | 不写业务，不直调 DAO |
| application | 业务编排→事务 | 事务内禁远程调用（Feign/MQ） |
| domain | 领域对象+Mapper 接口 | 新对象不含业务方法 |
| client | Feign+降级实现 | 不写业务 |
| listener | 反序列化→委托 application | 不写复杂业务 |
| infrastructure | DAO/Mapper 接口+实现 | — |
| query / utils | 查询层 / 工具类 | 不含业务状态 |

## 三、创建检查清单

- [ ] 层级正确？controller 不直调 DAO
- [ ] 事务内无远程调用？
- [ ] 类级 `@Transactional` 防 Feign 误入？
- [ ] bootstrap profile？默认 local，qa/prod 走 ctf-config
- [ ] parent POM？DC→ctf-parent / ADPT→adpt-parent
- [ ] SQL 表名带库名？`example_store.dc_xxx`（TICKET-002 教训）
- [ ] 构建走 `bash .claude/scripts/build.sh`（禁裸 mvn）
