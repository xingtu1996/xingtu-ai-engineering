# 方法：CLI 封装（CLI Encapsulation / Operation Twin Interface）

> 定义：把人工操作（控制台/手动命令）编程化为 **AI 可调用的受控 CLI**——动词式子命令 + 结构化输出 + 幂等 + 认证封装 + `--dry-run` + 人审门禁。这是本体论在 harness 自身的接口层：**名词**（参数/对象契约）+ **动词**（子命令=Action）+ **决策**（建议/执行/人审）。
> 渊源：阿里云 CLI / AWS CLI 实证（"终端完成控制台几乎所有操作，命令组合为脚本实现自动化运维"）；Palantir Foundry Action 受控写回；`工程实践-automation-ontology.md`。
> 关联：`scripts/_TEMPLATE.sh`（脚本骨架）/ `工程实践-automation-ontology.md`（三层映射）/ `理念-ontology.md`（三层理论）。

## 背书/引用备案（背书双轨）

**外部权威**：

| 引用 | 来源 | 一句话理念 | URL |
|------|------|-----------|-----|
| Alibaba Cloud CLI | 阿里云 官方 | 开源跨平台命令行工具，终端完成控制台几乎所有操作，命令组合为脚本实现自动化运维 | https://help.aliyun.com/zh/cli/product-overview/what-is-cli |
| AWS CLI | AWS 官方 | 命令行管理 AWS 资源，脚本化自动化运维 | https://aws.amazon.com/cli/ |
| Infrastructure as Code | Terraform/Argo CD | 部署/基础设施操作声明化、可编程化、可审计化（GitOps） | https://developer.hashicorp.com/terraform/docs |

**内部实证（示例企业落地）**：

- `sls-log.sh`（查日志）/ `rancher-tool.sh`（deployments/get/pods/logs/status/redeploy **动词式子命令**）/ `jenkins-ci-build.sh`（构建+`--watch`）/ `pma-sql.sh`（取数）
- `scripts/_TEMPLATE.sh` 骨架（`--dry-run`/trap ERR 行号/log 统一/超 50 行拆函数）

## 深入浅出

**一句话本质**：CLI = 操作孪生的接口层——把"人肉点控制台"变成 AI/脚本可调用的动词命令，**确定性、幂等、可审计、低 token**。

**反模式**：脚本只打印人类可读文本（AI 无法结构化消费）；无 `--dry-run`（危险操作不可预览）；无幂等键（重复触发出事）；认证硬编码（密钥泄露，违反 security-rules §二）；输出用裸 echo 无结构化 JSON。

## CLI vs Skill vs MCP（三接口定位）

| 接口 | 管什么 | 形态 | 适合 |
|------|--------|------|------|
| Skill | 怎么做（场景化流程打包） | 指令集 | 非确定性推理流程 |
| MCP | 外部工具/服务接入 | 工具协议 | 跨进程/第三方服务 |
| **CLI** | 示例企业自有运维操作封装 | 受控命令 | **确定性/幂等/可审计/低频高价值操作** |

## 封装骨架（本体论三层）

| 层 | 要素 | 落地要求 |
|----|------|---------|
| 语义层 | 参数契约 + 结构化输出 | 动词式子命令（get/list/logs/status/deploy/redeploy/diagnose）；`--json` 输出 `{status,decision,action,payload}`；`--help` 齐全；退出码规范（0 成功/非 0 失败） |
| 动力学层 | 动作执行封装 | 认证封装（env 文件 chmod 600，禁硬编码）；幂等键（commit SHA/构建号/业务 ID）；`--dry-run` 预览；超时 `-m`；重试；trap ERR 记失败行号 |
| 决策层 | 决策支持 | 只读默认（多读少写）；写操作 `--apply` 显式确认；生产级写默认 `--suggest` 输出 JSON 建议由人确认（HITL）；feature flag 控自治度；审计留痕（回写 CHANGELOG/索引） |

## 子命令命名约定

```
{domain}-tool.sh <verb> [target] [--ns X] [--json] [--dry-run] [--apply] [--suggest]
```

- verb 用动词：get/list/logs/status/deploy/redeploy/diagnose（参考 `rancher-tool.sh`）
- 读操作默认不写；写操作必须 `--apply` 或 `--suggest`+人审
- 执行后回写状态/结果（决策层闭环，见 `理念-local-loop.md` ⑥实战/⑦登记）

## 最小示例（可抄，按此骨架扩展）

```bash
#!/usr/bin/env bash
# mytool.sh — 动词式子命令 + --json + --dry-run + 幂等键
set -euo pipefail
CMD="${1:-help}"; shift || true; JSON=0; DRY=0
for a in "$@"; do case "$a" in
  --json) JSON=1;; --dry-run) DRY=1;; --apply) APPLY=1;;
  *) echo "未知参数: $a" >&2; exit 1;;
esac; done
emit() { # 结构化输出：语义层契约
  [ "$JSON" = "1" ] && echo "{\"status\":\"$1\",\"decision\":\"$2\",\"action\":\"$3\",\"payload\":\"$4\"}" || echo "$1 | $2 | $3 | $4"
}
case "$CMD" in
  list)
    # 只读操作默认不写（多读少写）
    emit ok view list "查询 10 条记录"
    ;;
  deploy)
    KEY="deploy-$(git rev-parse --short HEAD 2>/dev/null || echo manual)"  # 幂等键
    if [ "$DRY" = "1" ]; then emit dry plan deploy "KEY=$KEY（不执行）"
    elif [ "${APPLY:-0}" = "1" ]; then emit ok commit deploy "KEY=$KEY"
    else emit warn suggest deploy "需 --apply 确认（HITL 门禁）"; fi
    ;;
  help|*) echo "用法: mytool.sh <list|deploy> [--json] [--dry-run] [--apply]"; exit 0;;
esac
```

## 检查清单（封装前自问）

□ 能动词化吗（子命令）？□ 输出可 JSON 结构化吗（AI 可消费）？□ 认证走 env 不硬编码？□ 写操作有幂等键 + `--dry-run`？□ 生产写有人审门禁（`--suggest`/`--apply`）？□ 挂进 templates/README + CHANGELOG 注册？□ 先 grep `.claude/scripts` 复用已有不重复造（Ponytail Ladder 2）？

**版本历史**：2026-08-27 V1.0 新建（CLI 封装=本体论接口层：语义/动力学/决策三层骨架 + CLI vs Skill vs MCP 定位 + 最小示例），spec `2026082711-ontology-ai-era-enterprise` 产物；2026-08-27 V1.1 补最小可抄示例 + 版本历史改行内（对抗审查优化者建议）
