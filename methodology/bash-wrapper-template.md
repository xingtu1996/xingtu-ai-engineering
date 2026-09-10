# 方法：Bash 封装模板（AI 友好实操版）

> 定义：把人工操作封装成 **AI 友好、可复现的 bash 脚本**——纯文本交互 + `--json` 结构化 + 统一错误码 + 注释清晰 + 零依赖，让当前 Agent（Claude Code）能直接调用、可解析、可自愈。
> 渊源：`scripts/_TEMPLATE.sh`（骨架）+ `tool-automation-methodology.md`（五要素）+ `方法-cli-wrapper-template.md`（动词化理论层）+ fs-ops 实践（`fs-ops/commands/*.sh`）。
> 关联：`方法-cli-wrapper-template.md`（理论/动词化）｜ `scripts/_TEMPLATE.sh`（骨架）｜ `fs-ops/commands/_TEMPLATE.sh`（fs-ops 版）。

## 一句话本质

**脚本 = 人工操作的"AI 抓手"**——把"人肉点控制台"翻译成 Agent 能直接 shell 调用的命令。核心不是脚本本身，是**如何让 Agent 读懂并调用**。

## 定位：bash 是起步，CLI 是进阶（渐进路径）

> **bash 脚本 = 封装的第一级（起步）**，不是低配版，而是最轻的起点——零依赖、零框架、今天就能做。跑通价值后再进阶。

| 阶段 | 形态 | 门槛 | 适用 | 何时升级 |
|------|------|:---:|------|---------|
| **L1 起步** | bash 脚本（本模板） | **零**，今天能做 | 个人高频操作、单脚本 | 先用起来见利 |
| **L2 进阶** | CLI 封装（`方法-cli-wrapper-template.md`） | 中 | 团队共用、多子命令 | 2+ 人高频用、需动词化 |
| **L3 编排** | Skill 组合 | 低 | 多命令工作流 | 命令稳定后组合 |
| **L4 平台** | MCP Server | 高 | 外部 AI 平台接入 | 明确需求后（YAGNI） |

**演进原则**：**先 bash 后 CLI**——脚本跑通验证价值，再封装成 CLI（动词子命令 + 统一入口）；没跑通别急着上框架。bash 是入口，CLI/Skill/MCP 是锦上添花。

> 反模式：一上来就搭 CLI 框架（Cobra/picocli）——没验证价值就过度建设。先从一条 bash 命令开始，见利后再升级。

## 封装五步（人工操作 → AI 友好脚本）

| 步 | 动作 | 产出 |
|----|------|------|
| 1 | 识别人工高频操作（每周 ≥3 次） | 待封装操作清单 |
| 2 | 沉淀五要素（地址/用途/用法/认证/curl） | 操作说明书 |
| 3 | 人肉路径 → curl（F12 抓请求） | 可执行请求 |
| 4 | 套 `_TEMPLATE.sh` 骨架改业务 | 脚本 |
| 5 | **AI 友好化**（见下） | Agent 可直接调 |

## AI 友好化 5 要点（Agent 能否用，取决于此）

| # | 要素 | 做法 | 效果 |
|---|------|------|------|
| 1 | 纯文本交互 | 参数 + stdout | Agent shell 直接调 |
| 2 | `--json` 输出 | 结构化 | Agent 可解析 |
| 3 | 统一错误码 | `fs_err <code> <msg>`（2 用法/3 认证/4 未找到/5 上游） | Agent 自愈（code=3 去查认证） |
| 4 | 头部注释清晰 | 用法/环境/认证/依赖写全 | Agent 读注释即懂，他人可逆向 |
| 5 | 零依赖 | bash3.2 + python3 标准库 | 任何机器能跑 |

## 如何给当前 Agent 使用（作用机制）

```
人工操作五要素 → bash 脚本（_TEMPLATE 骨架）
  → ① toolbob 索引登记 → Agent 读到用法（shell 直接调）
  → ② 组合 Skill     → Agent 触发词唤起（多命令工作流）
  → ③ 统一 CLI       → Agent 一条命令调全能力（fs-ops）
```

- **方式 1 shell 直调**：`bash .claude/scripts/xxx.sh <args>`——Agent 靠 toolbox 索引 + 头部注释找到用法
- **方式 2 Skill 组合**：多脚本编成工作流，`.claude/skills/<name>/SKILL.md`，触发词唤起
- **方式 3 fs-ops CLI**：脚本升级为命令，`fs-ops <verb> --env <env>` 统一入口

## 最小示例（AI 友好版骨架）

```bash
#!/usr/bin/env bash
# mytool.sh — 查服务状态（AI 友好：--json + 错误码 + 注释）
# 用法: bash mytool.sh <APP> [--json]
# 认证: 零认证（示例）｜ 依赖: curl + python3
set -euo pipefail
APP="${1:-}"; [ -z "$APP" ] && { echo "用法: mytool.sh <APP> [--json]"; exit 2; }
JSON=0; for a in "$@"; do [ "$a" = "--json" ] && JSON=1; done
OUT="$(curl -sk -m 20 -H 'Accept: application/json' "https://api.example.com/apps/$APP" 2>/dev/null)"
if [ -z "$OUT" ]; then
  [ "$JSON" = "1" ] && echo '{"ok":false,"code":5,"message":"上游失败"}' || echo "❌ [code=5] 上游失败" >&2
  exit 5
fi
[ "$JSON" = "1" ] && echo "$OUT" || echo "$OUT" | python3 -m json.tool 2>/dev/null || echo "$OUT"
```

## 快速自查（封装前问自己）

```
□ 每周重复 ≥3 次？□ 五要素齐？□ --dry-run 预演？□ --json 输出？
□ 写守卫/确认有？□ 头部注释清晰？□ toolbox 登记？□ 已 grep 复用不重复造？
```

## 版本历史
| 日期 | 变更 |
|------|------|
| 2026-08-27 | 新建：AI 友好 bash 封装实操模板（五步 + AI 友好 5 要点 + 三种 Agent 使用方式） |
