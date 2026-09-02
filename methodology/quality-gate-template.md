---
name: 方法-质量门禁模板
description: 质量门禁模板（Quality Gates）——L1~L6 提交门禁清单，提交前逐项核查
---

# 方法-质量门禁（Quality Gates）

> 定义：**质量门禁（Quality Gates）** = 提交前用机器 + 清单强制拦截质量红线的关卡。未过门禁不进主干。

## 一、深入浅出

**一句话本质**：门禁 = 提交前机器/清单强制拦质量红线，不是自觉承诺。

**反模式**：① 跳门禁（`--no-verify`）→ 事故漏网；② 口头承诺无证据 → 无法复核；③ 形同虚设（空 check / 过不了就删）→ 更糟。

## 二、测试金字塔 6 层门禁表

| 层 | 门禁 | 工具 | 通过标准 |
|:--:|------|------|---------|
| L6 | 安全 | dependency-check | 高危 CVE 清零 |
| L5 | 规范/静态 | SonarQube | 无 Blocker |
| L4 | 变异 | PIT | 变异存活率 < 阈值 |
| L3 | E2E | curl 全链路 | 关键链路全通 |
| L2 | API 契约 | validator curl | ≥3 条全过 |
| L1 | 单元 | JUnit5 | Service ≥85% |

## 三、示例企业实践（引用不复制）

| 门禁 | 示例企业落地 | 位置 |
|------|---------|------|
| Constitution Check | 禁止模式 §六 无命中 | `constitution.md` §七 |
| 安全三条件 | 退出/幂等/回滚（计费/库存/权限必填） | 同 §七 |
| 级联影响 | CBM trace_path 3 跳 + grep 补漏 | `CLAUDE.md` 零-B |
| L1/L2 | Service ≥85% + curl ≥3 | `specs-rules.md` §九 |
| 对抗审查 | 三角色 Refuter/Optimizer/Auditor 无 🔴 | `skills/adversarial-review` |
| 六层 Token | rtk + Caveman + Headroom 已激活 | `CLAUDE.md` 六层 |

## 四、骨架：提交前逐项勾选

```
□ Constitution Check（§六 无命中）
□ 安全三条件（退出/幂等/回滚；计费/库存/权限必填）
□ 级联影响（CBM 3 跳已查；高警戒全量 grep）
□ L1 ≥85% | L2 curl ≥3 全过 | L3 E2E 关键链路
□ L4 PIT | L5 Sonar 无 Blocker | L6 无高危 CVE
□ 对抗审查三角色通过，无 🔴
□ 六层 Token 激活（CBM + rtk + Caveman + Headroom）
□ 编译零错误（build.sh）| 测试后进程已清理（RED-6）
```

## 五、检查清单

- [ ] L1~L6 每层门禁全过
- [ ] 安全三条件明确（计费/库存/权限必填）
- [ ] 级联影响 3 跳已查（CBM + grep）
- [ ] 对抗审查无 🔴 阻塞项
- [ ] 未跳门禁（无 `--no-verify` / 无空断言 / 证据可复核）

## 六、关联

`constitution.md` §七 ｜ `specs-rules.md` §九 ｜ `templates/mr-template.md` ｜ `skills/adversarial-review/SKILL.md`

## 七、引用备案（背书双轨）

**外部权威**：

| 权威来源 | 作者/机构 | 年份 | URL | 一句话理念 |
|---------|---------|:---:|------|-----------|
| 《Succeeding with Agile》测试金字塔 | Mike Cohn | 2009 | https://martinfowler.com/bliki/TestPyramid.html | 测试金字塔出处：底部大量快速单元测试、顶部少量慢 E2E（对应 L1~L6 分层原则） |
| Google Testing Blog《Just Say No to More End-to-End Tests》 | Google | 2015 | https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html | E2E 泛滥警示 + 70/20/10 比例参考（对应"E2E 只测关键链路"，倒置金字塔=CI 慢主因） |

**内部实证**：

| 案例 | 位置 | 证明什么 |
|------|------|---------|
| 质量门禁提交前必查清单 | `rules/constitution.md` §七（Constitution Check/安全三条件/级联影响/测试金字塔/对抗审查/Token） | 门禁规范已入宪法，机器可查、提交前强制 |
| 六层质量防线全景 | `specs/2026081716-AI-TESTING-PLAYBOOK/deliverables/ai-testing-playbook/质量体系全景.html`（L6 安全→L1 单元） | 金字塔 L1~L6 在示例企业测试体系中的全景落地 |
| 对抗审查 Skill | `.claude/skills/adversarial-review/SKILL.md`（Refuter/Optimizer/Auditor 三角色） | 门禁"对抗审查"项的真实 Skill，非口号 |
| 物料二期实战 | `specs/物料二期-实战沉淀-可复用资产_20260803.md`（质量门禁在 TICKET-002/1909/1911 落地运用） | 门禁在真实项目中的实战证据 |
