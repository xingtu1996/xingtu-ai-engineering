# 方法-部署 SOP 模板（Deployment SOP）

> 用途：把"新版本安全放到目标环境"固化为可重复执行的受控变更 SOP，AI 有样学样。
> 引用：`templates/sop-template.md`（必带案例铁律）｜`research/20260812-fs-deployment-sop.md` §七（UAT gitops 实测实例）
> 体积：约 2.3KB

## 定义
**部署 SOP（Deployment SOP）**＝把部署过程固化为受控变更清单：前置→步骤→验证→回滚四件套，可重复、可验证、可回退。

## 深入浅出
**本质：部署 = 受控变更**，四件套缺一不可——前置条件 → 部署步骤 → 验证清单 → 回滚方案。

**反模式**：
- 部署无回滚方案（出问题只能硬修）
- 环境差异不标注（把 SIT 步骤当 UAT 用，改错仓库/连错库）
- 跳过验证（Build SUCCESS ≠ 发布完成，需 ArgoCD 滚动完成 + 业务冒烟）

## 部署 SOP 骨架

| 节 | 必含 | 实例 |
|----|------|------|
| 环境 | 目标环境/仓库/凭据/tag 格式 | config-fs-argocd-uat；tag=`{分支}-{版本}-{commit7}` |
| 前置条件 | 缺一项即终止 | 镜像 tag 已在 Harbor/ACR，否则 ArgoCD 拉不到 |
| 部署步骤 | 每步 操作/命令/验证/产出 | git pull → 改 values Tag → commit+push |
| 验证清单 | 发布完成判定标准 | Rancher Progressing=False + availableReplicas；Kibana Tomcat started |
| 回滚方案 | 出问题如何回退 | git revert values 旧 tag → push 触发回滚 |
| 与它环境区别 | 对比表 | SIT=Jenkins 自动改 vs UAT=手动改 values |

## SOP 必带案例铁律（引用 SOP模板）
**每阶段必须附一条真实案例（`例：…`），禁止空骨架。** 先找案例（research/incidents/specs）再套骨架。来源：`templates/sop-template.md`。

## 示例企业实践
- **gitops 手动发布**：改 values 镜像 tag → push → ArgoCD 滚动发布（2026-08-24 dc-refund UAT 实测）
- **部署顺序**：先 pull 再改（防覆盖他人）→ 一次一个服务 → 先确认 tag 已构建
- **回滚**：git revert values 到旧 tag + push，ArgoCD 回滚旧版本

## 检查清单
- [ ] 前置条件齐？（环境/凭据/镜像 tag 已在）
- [ ] 步骤可执行？（每步有命令 + 验证断言）
- [ ] 验证清单能判过？（发布完成判定标准明确）
- [ ] 回滚方案有？（出问题回退到哪个 tag）
- [ ] 环境区别标注？（SIT/UAT/prod 差异表）

## 引用备案

### 外部权威（方法论可信度背书）

| 来源 | 作者 | 年份 | URL | 背书要点 |
|------|------|:---:|------|---------|
| 《Site Reliability Engineering》(SRE 手册) | Google Beyer 等 | 2016 | https://sre.google/sre-book/ | 受控变更与回滚：错误预算控制发布节奏、渐进/金丝雀发布、自动回滚；"未测试回滚机制导致事故"教训 → 回滚方案必须可测 |
| The Twelve-Factor App — X. Dev/prod parity | Adam Wiggins | 2011 | https://12factor.net/dev-prod-parity | 开发/预发/生产尽可能一致（时间/人员/工具三缺口）→ 对应"环境区别不标注"反模式 |

### 内部实证（示例企业真实用过）

| 落点 | 类型 | 链接 | 实证内容 |
|------|------|------|---------|
| `research/20260812-fs-deployment-sop.md` | 调研/实证 | `.claude/research/20260812-fs-deployment-sop.md` | TICKET-002 dc-promotion SIT 部署实测（Jenkins #473 完整日志 + Rancher + Kibana 三视角验证），部署架构+可 AI 托管环节 |
| 模板内实例 | 实战 | 本文档"示例企业实践"节 | 2026-08-24 dc-refund UAT gitops 手动发布：改 values 镜像 tag → push → ArgoCD 滚动发布 |
