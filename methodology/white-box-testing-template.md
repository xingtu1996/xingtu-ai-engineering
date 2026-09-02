# 方法-白盒测试（White-box Testing）

> 定义：**白盒测试（White-box Testing）** = 代码级测试：基于代码内部结构（分支/路径/边界）设计用例，验证"实现对不对"，对应测试金字塔 L1（单元测试）。出处：Myers《The Art of Software Testing》(1979)。

## 一、深入浅出

**一句话本质**：看代码内部逻辑测——分支、边界、异常路径全走一遍，测"实现对不对"，不是只测"功能能不能用"。

**反模式**：只测 happy path 不测分支（边界裸奔）｜ mock 全依赖｜ 只 assertNotNull｜ 补测试刷覆盖率。

## 二、示例企业实践对应（引用不复制）

| 白盒要素 | 示例企业落地 | 位置 |
|---------|---------|------|
| JUnit5 + Service ≥85% | L1 单元测试 | CLAUDE.md 测试金字塔 |
| TDD 红绿重构 | tasks.md Phase 2 a🔴→b🟢→c🔵 | `specs-rules.md` §5.5 |
| 本地测试左移 | RED-8：编译→本地→curl→合并 | `constitution.md` §三 |
| Java 三层骨架 | Service/Mapper/Controller tmpl | `.claude/test/templates/` |

**tmpl**：`ServiceTest.java.tmpl`（Mockito 打桩）｜ `MapperTest.java.tmpl`（MybatisTest 测 SQL）｜ `ControllerTest.java.tmpl`（WebMvcTest 测 HTTP）。

## 三、骨架：用例设计 + @Test 断言模式

```
用例四类（按逻辑补全）：
① 正常 典型输入 → assertEquals + verify
② 异常 非法参数 → assertThrows 指定异常
③ 边界 0/空/最大/精度 → 断临界行为
④ 分支 每分支一用例 → 断分支结果
```

```java
@Test void deduct_whenValid() {
    when(mapper.selectById(any())).thenReturn(stock);
    assertEquals(50, service.deduct(id, 10).getRemain());
    verify(mapper, times(1)).update(any());
}
@Test void deduct_whenQtyZero() {
    assertThrows(BusinessException.class, () -> service.deduct(id, 0));
}
@Test void deduct_whenDepFails() {
    when(mapper.update(any())).thenThrow(RuntimeException.class);
    assertThrows(RuntimeException.class, () -> service.deduct(id, 1));
}
```

## 四、检查清单

- [ ] 分支覆盖：每个 if/else/switch 至少一用例
- [ ] 异常路径：参数非法 + 依赖失败传播
- [ ] 边界值：0/空/最大/金额精度；断言验值 + verify 交互
- [ ] 覆盖率 Service ≥85%；先写测试后实现（🔴→🟢→🔵）

## 五、关联

`test/templates/` 三层 tmpl ｜ `工程实践-TDD.md` ｜ `specs-rules.md` §5.5 ｜ `constitution.md` RED-8
