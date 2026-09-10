# 方法：渐进式加载（Progressive Disclosure / Just-in-time Loading）

> 定义：harness 上下文按需加载——常驻只放高信号指针，细节触发时才读全文。预算/证据见 `harness-size-standard.md`（每 KB 常驻=上下文税，>64K token 性能降 30%+）。
> 引用备案：
> - Anthropic《Effective context engineering for AI agents》(2025)：just-in-time=维护轻量标识（路径/查询）、运行时工具动态加载；progressive disclosure=逐层探索发现相关上下文。 https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
> - mcpjam《Progressive Disclosure Might Replace MCP》：Claude Skills 最小 SKILL.md 起步、需时再读引用，区别于 MCP 全量预载。 https://www.mcpjam.com/blog/claude-agent-skills

## 深入浅出
- 一句话本质：只加载当下需要的，其余按需取；每 KB 常驻=固定烧 token+持续性能退化。
- 反模式：①全量常驻（400+ 行 kitchen sink，选择性忽略）；②一次性预载塞满窗口（technically full, practically empty）；③用后不释放（长会话膨胀）。

## 加载分层模型
| 层 | 内容 | 载入时机 |
|----|------|---------|
| L1 常驻 | CLAUDE.md/rules 精简高信号（< 窗口 5%） | 会话常驻 |
| L2 索引指针 | 渐进披露表/MEMORY.md/目录README（~50-100 token/条） | 常驻或首查 |
| L3 按需全文 | Skill/模板/触发规则/清单全文 | 触发才读 |

## 渐进式加载骨架
```
常驻极简 → 指针索引 → 触发词路由 → 按需取全文 → 用后释放
```

## 示例企业实践对应
- 渐进式披露表：CLAUDE.md「渐进式披露—自动加载的规则/清单」表
- Skill 自动调用：CLAUDE.md「Skill 自动调用」表（场景→Skill，命中才加载）
- rules 触发词：mq/zeebe/容量 命中才读（场景触发 rules 表）
- templates 按需：`templates/README.md` 家族索引取样板
- CBM 按需索引：TOK-006 索引有效即用、过期增量、禁每会话全量
- L2 指针：knowledge-map / MEMORY.md 知识只记一遍、先查后写

## 检查清单
- □ 这条常驻必要吗？（删掉 AI 会犯错吗？不会 → 降 L2/L3）
- □ 有索引指针吗？（L2 表一行：名+触发场景+内容）
- □ 触发路由了吗？（触发词/场景明确指向，非模糊搜索）
- □ 释放了吗？（用后即弃，不保留到长会话）
- □ 体积过预算吗？（常驻 < 窗口 5%，harness-size.sh -g）
