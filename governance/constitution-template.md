# 宪法模板（Constitution / Governance Baseline）

> 新项目/积木包治理底线骨架。基准：示例企业 `rules/constitution.md` + `harness-gate.md`。步骤：定阶段→取红线集→填项目红线→挂机械拦截→过体积门禁。

## 一、深入浅出

**宪法=治理底线：机器可执行的红线先于散文规则。**
反模式：①只写愿望不写红线；②红线无机械拦截（规则是愿望，hook 才是墙）；③红线泛滥无门禁。

## 二、示例企业宪法骨架（引用不复制）

### 铁律（三条）
| # | 铁律 | 一句话 |
|---|------|--------|
| 1 | No Spec No Code | 文档未完成禁写码 |
| 2 | Spec is Truth | 文档冲突→错的是代码 |
| 3 | Reverse Sync | 发现 Bug→先修文档再修码 |

### RED 红线（示例，按项目增删）
| # | 红线 |
|---|------|
| RED-1 | 禁 `rm -rf`（任何场景） |
| RED-2 | 禁批量删非空目录，优先 `mv` |
| RED-3 | 删除前查 git 状态 |
| RED-4 | 禁覆盖/重构已有功能（追加式） |

### 禁止模式（≥10 条，每条给替代）
空 catch→log.error｜SQL `${}`→`#{}`｜事务内远程调用→事务外/MQ｜消费者不幂等→幂等键｜无分页→强制分页｜密钥明文→环境变量｜日志敏感明文→脱敏｜循环内远程调用→批量｜无回滚→回滚方案｜硬编码容量→标设计寿命

### 安全三条件（计费/库存/权限必填）
退出（可中止）｜幂等（重入安全）｜回滚（失败可恢复）

### 质量门禁（提交前）
宪法检查｜级联影响｜测试金字塔 L1≥85% / L2 curl≥3｜对抗审查

## 三、积木化宪法（按阶段裁剪）

| 版 | 适用 | 保留 | 裁剪 |
|----|------|------|------|
| S 售前 | POC/评估 | RED-1~3+禁写码 | 铁律/禁止模式/门禁 |
| M 新建 | 模块化单体 | 铁律+RED+禁止模式+安全三条件+轻门禁 | 级联影响/契约 |
| L 存量 | 5+ 微服务 | 全量 | 无 |

## 四、检查清单

- [ ] 铁律有？
- [ ] 红线机械可拦（hook/脚本/Gate）？
- [ ] 禁止模式≥10 条且有替代？
- [ ] 安全三条件（计费/库存/权限）？
- [ ] 门禁有？
- [ ] 体积<2.5KB（随阶段裁剪）？

## 引用备案（背书双轨）

**外部权威**：

| 权威来源 | 作者/年 | URL | 要点 |
|---------|--------|-----|------|
| 《Constitutional AI: Harmlessness from AI Feedback》 | Bai et al.（Anthropic）2022 | https://arxiv.org/abs/2212.08073 | 「宪法 = 约束模型/Agent 行为的基本原则集」的学术源头——示例企业宪法"机器可执行红线先于散文规则"同构 |
| CIO《Why your 2026 IT strategy needs an agentic constitution》 | CIO.com 2026 | https://www.cio.com/article/4118138/why-your-2026-it-strategy-needs-an-agentic-constitution.html | Agent 治理最佳实践：宪法 = 机器可读基础原则 + 红线分级（Tier 3 = 任何 Agent 不可自主执行）——与本文 RED 红线 + S/M/L 分级裁剪同构 |
| 《A Constitution for One》 | GitHub 实践 2026 | https://github.com/Chong169/a-constitution-for-one | 7 个月零事故实证：观察先行再立法 + 不可逆操作人把关 + 追加式账本——"红线机械可拦"的经验佐证 |

**内部实证（示例企业落地）**：

- `.claude/rules/constitution.md` — 示例企业宪法本体全量实证：三条铁律（No Spec No Code / Spec is Truth / Reverse Sync）+ RED-1~8 红线 + §六 禁止模式 10 条 + §七 质量门禁
- `.claude/rules/harness-gate.md` — 机械门禁实证：准入五问 + `harness-size.sh -g` 体积门禁 = "红线机械可拦"（hook/脚本/Gate 是墙，散文规则只是愿望）
- 宪法模板 §二 骨架直接引用 constitution.md（引用不复制）— 宪法即"从示例企业宪法裁剪出积木包治理底线"

## 版本历史

| 日期 | 版本 | 变更 | 变更人 |
|------|:---:|------|--------|
| 2026-08-25 | V1.0 | 新建：治理底线骨架 + S/M/L 裁剪 | Claude Code（操盘：XingTu） |
