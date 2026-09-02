---
name: 方法-Git分支管理
description: Git 分支管理——发布节奏隔离 + 建分支 SOP
---

# 方法-Git分支管理（Git Branching Model）

> 定义：**Git 分支管理模型** = 用分支隔离发布节奏：常驻分支承载稳定/集成线，短命分支承载特性/修复，合并路径决定发布节奏。
> 引用：Driessen《A Successful Git Branching Model》(2010)；Hammant《Trunk-Based Development》(2013)；GitHub Flow 文档。

## 一、三种模型对比

| 维度 | Git Flow | Trunk-based | GitHub Flow |
|------|:---:|:---:|:---:|
| 常驻分支 | master+develop | 单主干 | 单 main |
| 特性分支 | 数周 | 1-2 天 | PR 即删 |
| 发布 | release+tag | 主干打 tag | main 即发布 |
| 集成频率 | 特性结束才合 | 每日多次 | 每次 PR |
| 冲突 | 频繁痛 | 少小 | 少 |
| 适用 | 版本化发布/多版本 | Web/SaaS/CD | 单线快速发布 |

> 反思：Git Flow 非银弹；Google 95% 单主干。

## 二、深入浅出

**一句话本质**：分支 = 发布节奏的隔离——从哪建/何时合，决定发布节奏与冲突成本。

**反模式（RED-7）**：从落后本地分支建分支——把旧基线带进新功能 → 合并冲突/返工。

## 三、示例企业分支模型

| 分支 | 定位 | 依据 |
|------|------|------|
| `main` | 主分支，生产就绪 | git-rules |
| `qa` | 集成基线，new-branch.sh 默认 `origin/qa` | new-branch.sh |
| `qa-wl` | QA/生产修复分支（当前） | git-rules |
| `qa-sit`/`qa-uat` | 环境部署分支，待确认 | 部署惯例推断 |
| `dev-FS-*`/`fix/FS-*` | 短命开发/修复，从 origin/qa 建 | new-branch.sh |

## 四、建分支 SOP（RED-7）

```bash
# 一律走脚本，禁手动 git checkout -b（BCI-001）
bash .claude/scripts/new-branch.sh dev-TICKET-002            # 默认 origin/qa
bash .claude/scripts/new-branch.sh fix/TICKET-001 --carry   # 携带未提交修改
bash .claude/scripts/new-branch.sh dev-TICKET-002 main       # 指定 origin/main
```

流程：① 工作区干净（脏则拒/--carry）→ ② 重名校验 → ③ fetch origin/<base> → ④ checkout -b 于 origin/<base> → ⑤ 输出基线证据。

**基线落后自检**（期望 `0 <N>`）：
```bash
git rev-list --left-right --count origin/qa...HEAD   # 左=落后，右=领先
```

## 五、commit 规范

```
t-[TICKET]-[SUBTASK]-[DESCRIPTION]-[AUTHOR]
# t-[履约-4.10.05.05]-[TICKET-002]-[赠品/物料出库履约-物料匹配异常特性支持]-[XingTu]
```
SUBTASK：`TICKET-XXX`/`CTF-XXXXX`/`dev-TICKET-XXX`；方括号禁空格；禁 Co-Authored-By/--no-verify/--force。

## 六、合并/基线同步

```bash
git fetch origin <base> && git merge origin/<base>   # 功能分支同步基线
git branch -f <base> origin/<base>                    # 本地基线同步
# MR：source=<分支> → target=<base>
```

## 七、关联

`rules/git-rules.md` §二/§四 ｜ `constitution.md` §三 RED-7 ｜ `scripts/new-branch.sh` ｜ `memory/bad-case-index.md` BCI-001

## 八、引用备案

### 外部权威（方法论可信度背书）

| 来源 | 作者 | 年份 | URL | 背书要点 |
|------|------|:---:|------|---------|
| 《A Successful Git Branching Model》(Git Flow 原始文) | Vincent Driessen | 2010 | https://nvie.com/posts/a-successful-git-branching-model/ | Git Flow 定义（master+develop+feature/release/hotfix）；含 2020 反思：web/持续交付场景应走简化流程，**非银弹** → 对应本文"反思：Git Flow 非银弹" |
| Trunk-Based Development | Paul Hammant | 2013+ | https://trunkbaseddevelopment.com/ | 主干开发实践（Google 95% 单主干佐证）；短命分支 1-2 天合并 → 对应对比表"Trunk-based"列 |

### 内部实证（示例企业真实用过）

| 落点 | 类型 | 链接 | 实证内容 |
|------|------|------|---------|
| `scripts/new-branch.sh` | 脚本 | `.claude/scripts/new-branch.sh` | RED-7 建分支 SOP 落地：自动 fetch + 从 origin/qa 远端 ref 创建 + 基线 commit 证据 |
| `rules/git-rules.md` §二/§四 | 规则 | `.claude/rules/git-rules.md` | 分支策略（main/qa/qa-wl/dev-FS-*）+ 新建分支规范（禁手动 checkout -b） |
| `rules/constitution.md` §三 RED-7 | 红线 | `.claude/rules/constitution.md` | 禁止从本地落后分支建分支，统一走 new-branch.sh |
| `memory/bad-case-index.md` BCI-001（用户级 auto-memory） | 案例 | `~/.claude/projects/<project>/memory/bad-case-index.md` | 功能分支基线落后 284 commits 导致启动失败实证（RED-7 的由来） |
