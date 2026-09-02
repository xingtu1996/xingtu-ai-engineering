# 方法：Git Worktree 并行（Git Worktree）

> 定义：同一仓库（共享 .git/objects）挂多个独立工作树，各占独立目录/分支/编译产物。
> 引用：Git 官方 `git-worktree(1)`；spec `2026072816-worktree-vs-ai-planning`（8/04 修正）；CLAUDE.md L87。

## 一、本质

**同仓库多工作树并行 = 空间换时间**：把"人同时改多文件"的物理冲突用目录隔离推迟，互不抢 checkout。
**反模式（AI 滥用）**：规划/读码/写文档用 worktree = 杀鸡用牛刀（建树/清树/依赖重复成本高）。

> 铁律：worktree=空间换时间（给人）；AI=时间换空间，先规划再并行。

## 二、场景判断表

| 场景 | 用？ | 依据 |
|------|:---:|------|
| **多人并行改同仓库**（不同分支） | ✅ | 官方 emergency-fix 例：插队修复互不打断 |
| **AI 多 Spec 并行（规划）** | ❌ | CLAUDE.md L87：Spec+Subagent 上下文隔离已避免冲突，合并成本零 |
| **AI 编码多会话写代码** | ✅ | 8/04 修正：规划不需 worktree，编码需——多会话改文件须物理隔离 |
| 同一文件多人改 | ⚠️ | 仍 merge conflict |

## 三、操作 SOP

### 1. 创建（建分支+建树一步，等价 new-branch.sh 基线保证）
```bash
git fetch origin qa
git worktree add -b dev-TICKET-002 .worktrees/worktree-001 origin/qa   # 基线=远端 origin/qa（RED-7）
cd .worktrees/worktree-001 && bash ../.claude/scripts/build.sh clean install
```

### 2. 查看 / 切换
```bash
git worktree list   # 全部工作树（主树第一）；checkout 只切该树分支
```

### 3. 清理（红线：禁 rm -rf，用 git worktree remove）
```bash
cd <主仓库> && git worktree remove .worktrees/worktree-001   # 有未提交改动会拒绝，先保存
git branch -d dev-TICKET-002                              # remove 不删分支，单独删
git worktree prune                                     # 手工删目录后清管理文件
```

### 4. 与 new-branch.sh 结合
- 主树已建分支：先切回原分支（同分支不能挂两树），再 `git worktree add <path> <branch>`。
- 推荐直接走步骤 1：一条命令等价 new-branch.sh"远端基线+拒重名"。

## 四、注意事项
- 同分支不能同时 checkout 两树（除非 --force）；各树独立 target/，Maven 共享 .m2；建树目录用 `.worktrees/<spec>`。

## 版本历史
| 日期 | 版本 | 变更 |
|------|:---:|------|
| 2026-08-25 | V1.0 | 新建：场景判断表 + 创建/清理 SOP |

## 五、引用备案

### 外部权威（方法论可信度背书）

| 来源 | 作者 | 年份 | URL | 背书要点 |
|------|------|:---:|------|---------|
| Git 官方 `git-worktree(1)` 文档 | Git 项目 | 持续更新 | https://git-scm.com/docs/git-worktree | 多工作树权威语义：add/list/lock/move/remove/prune、`-b <branch>` 一步建分支建树、refs 共享规则、worktreeConfig 隔离 → 对应操作 SOP 全部命令的官方依据 |

### 内部实证（示例企业真实用过）

| 落点 | 类型 | 链接 | 实证内容 |
|------|------|------|---------|
| `specs/2026072816-worktree-vs-ai-planning/` | Spec 调研 | `.claude/specs/2026072816-worktree-vs-ai-planning/` | worktree vs AI 规划实证对比（8/04 修正）：规划不需 worktree、多会话编码需物理隔离 → 对应场景判断表 |
| `scripts/new-branch.sh` | 脚本 | `.claude/scripts/new-branch.sh` | 步骤 1 `git worktree add -b ... origin/qa` 一条命令等价"远端基线+拒重名"（RED-7 基线保证） |
| `workflows/07-multi-session-parallel-exec.md` | 工作流 | `.claude/workflows/07-multi-session-parallel-exec.md` | 多会话并行执行落点（编码场景需 worktree 隔离） |
