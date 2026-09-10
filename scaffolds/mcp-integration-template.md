# MCP 接入样板

> 用途：扩展 Claude 工具，补内置不足。落地项目根 `.mcp.json`（gitignore 护密钥）。
> 接入步骤：选型 → 配置 .mcp.json → 验证 → 登记 CHANGELOG

## MCP 类型表
| 类型 | 配置 | 场景 |
|------|------|------|
| 官方 | `type:"http"` + 官方 url + headers | 官方托管 SaaS |
| npm | `type:"stdio"` + `command:"npx"` + args | 官方/社区 JS 生态（playwright） |
| 本地 | `type:"stdio"` + command + args + env | 自有脚本封装，不出内网 |
| 远程 | `type:"sse"` + url | 自建远程服务 |

## 配置样板（.mcp.json）
```json
{
  "mcpServers": {
    "tencent-docs": {
      "type": "http",
      "url": "https://docs.qq.com/openapi/mcp",
      "headers": { "Authorization": "$TENCENT_DOCS_TOKEN" }
    },
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--isolated"],
      "env": {}
    }
  }
}
```
stdio 用 command/args/env，HTTP 用 url/headers；密钥走 env 禁明文；项目级 `.mcp.json`，个人级 `claude mcp add --scope user`。

## 准入检查（任一「是」→ 拒）
| 检查 | 来源 | 判定 |
|------|------|------|
| 现有组件已覆盖？ | harness-gate 五问-a | 拒 |
| 模型已知 L0/L1？ | harness-gate 五问-b | 拒 |
| 体积超标/无验收回归？ | harness-gate 五问-c/e | 拒 |
| 30 天触发 <3 次？ | harness-gate 五问-d | 观察期 |
| 来源信任分级？ | skills-governance §一 | 官方>已验证>社区 |
| 安全审查？ | skills-governance §四 | 注入/外泄/破坏性/`!` 命令 |

## 接入骨架
```
1 目的: 解决什么工具缺口
2 类型: 按上表选官方/npm/本地/远程
3 准入: 五问全否 + 来源可信 + 安全通过
4 配置: 写 .mcp.json（密钥走 env）
5 验证: claude mcp list → 工具冒烟
6 登记: CHANGELOG + toolbox/ + mcp/README
```

## 验证命令
```bash
claude mcp list | grep <server-name>   # 确认注册
# 冒烟: curl tools/list 或对话触发工具
```
