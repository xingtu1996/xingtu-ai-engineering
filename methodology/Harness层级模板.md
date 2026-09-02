# 方法：Harness 层级（Harness Layering）

> 定义：harness 按"管什么"分层——L0~L5 架构分层 + 执行/元层组件分工 + 六层 Token + L1~L3 加载。新人按此顺序理解，可快速定位组件"放哪、归谁管、何时加载"。
> 引用：全景 §二（`research/2026082514-harness-panorama-methodology/00_README.md`）｜templates/README｜CLAUDE.md 零-D

## 深入浅出
- 本质：harness 分执行层（管运行时：规则/技能/角色/流程）与元层（管产物形状：模板/契约/Spec）；横按 L0~L5 分层、L1~L3 控加载。
- 反模式：①全塞常驻（烧上下文税）；②层级混乱（归位错）；③元层自动路由（违反 0 常驻）。

## 架构分层 L0~L5（引用全景 §二）
```
L5 产品层 HARNESS.md → 总纲与导航
L4 编排层 workflows/ → 流程 SOP
L3 执行层 agents/ + skills/ → 独立角色 + 场景化打包
L2 治理层 rules/ → 治理护栏（禁行为）
L1 知识层 memory/ + incidents/ → 知识积累与生产案例
L0 配置层 config.yaml → 工具/技术栈/账号
```

## 执行层 vs 元层（组件分工）
| 层 | 组件 | 管什么 | 加载 |
|----|------|--------|------|
| 执行 | Rule | 禁行为（红线） | 常驻或触发词 |
| 执行 | Skill | 怎么做（场景化流程） | 触发词路由 |
| 执行 | Agent/Workflow/MCP | 执行/编排/外部工具 | 按需调用 |
| 元层 | Template | 产物形状（骨架+占位） | 按需索取，0 常驻 |
| 元层 | Contract/Spec | 握手契约/静态真相源 | 变更时校验 |

## 六层 Token（引用 CLAUDE.md 零-D）
```
上行: 需求→检索→协议→输入压缩→模型 ｜ 下行: 模型→散文→生成
```
需求先文档、检索少查、协议压命令、输入压缩源裁剪、散文精简输出、生成最少代码。

## 加载分层（引用方法-渐进式加载）
| 层 | 内容 | 时机 |
|----|------|------|
| L1 常驻 | CLAUDE.md/核心 rules（<窗口 5%） | 会话常驻 |
| L2 指针 | knowledge-map/templates-README/MEMORY | 常驻或首查 |
| L3 全文 | Skill/模板/触发规则全文 | 触发才读 |

## 骨架（新人理解顺序）
```
① L0~L5 目录定位 → ② 执行 vs 元层 → ③ 六层 Token → ④ L1~L3 加载
```

## 检查清单
- □ 新组件可归位（层+加载体）？常驻最小化（模板 0 常驻）？元层不自动执行？
- □ 六层 Token 激活（spec/CBM/rtk/Headroom/Caveman/Ponytail）？size.sh -g 过预算？

## 引用备案

### 外部权威（方法论可信度背书）

| 来源 | 作者 | 年份 | URL | 背书要点 |
|------|------|:---:|------|---------|
| 《Effective Harnesses for Long-Running Agents》 | Anthropic Engineering | 2025-11-26 | https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents | harness = 长运行 Agent 的骨架：初始 agent + 编码 agent 交接、进度文件、默认 FAIL 契约 → 佐证"执行层管运行时 / 元层管产物形状"分层 |
| anthropics/cwc-long-running-agents | Anthropic 官方仓库 | 2025 | https://github.com/anthropics/cwc-long-running-agents | harness 原语落地：Default-FAIL 契约、Fresh-context evaluator、Agent 维护的交接（git 提交+进度）→ 与示例企业 Spec/契约/断点续传同构 |

### 内部实证（示例企业真实用过）

| 落点 | 类型 | 链接 | 实证内容 |
|------|------|------|---------|
| `research/2026082514-harness-panorama-methodology/00_README.md` | 调研 | `.claude/research/2026082514-harness-panorama-methodology/00_README.md` | 全景方法论：L0~L5 架构分层 + 执行/元层分工出处（本模板"引用全景 §二"指向处） |
| `rules/harness-size-standard.md` | 规则 | `.claude/rules/harness-size-standard.md` | 体积预算 + 上下文管理映射（常驻 token / L1~L3 加载依据） |
| `rules/harness-philosophy.md` | 规则 | `.claude/rules/harness-philosophy.md` | 精神层元原则（真相第一/简单即真/复利进化/协作即对抗）——分层背后的治理哲学 |
