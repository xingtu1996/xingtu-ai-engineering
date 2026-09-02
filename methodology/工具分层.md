# 方法：AI 编程工具分层（AI Coding Tool Tiers）

> 按「用户工程能力 + 约束需求」选 AI 编程工具层级的决策方法。
> 溯源：`research/2026082514-harness-panorama-methodology/00_README.md` + 2026-08 WebSearch（Omarchy/dsh/Codex Harness 开源）。

## 定义

工具分层 = 把 AI 编程工具按「**能力/掌控度从低到高**」划为 **L0~L4 五层**（单调递增）。**层越高能力越强、工程门槛越高**——不是越高越好，匹配才有效。

| 层 | 名称 | 抽象层级 | 能力轴 |
|----|------|---------|-------|
| L0 | 裸聊 chatbot | 无工具/无工程 | 最低 |
| L1 | 外行低代码 | 有抽象工具，非程序员可用 | 低 |
| L2 | 中间 IDE | 半专业协作 | 中 |
| L3 | 专业 CLI | 专业受控执行 | 高 |
| L4 | 操作系统级 harness | 顶级自建环境 | 最高 |

## 引用备案

- 极客公园《硅谷押注的下一个 Harness，是整个桌面操作系统》— geekpark.net/news/369298
- InfoQ《The Open-Sourcing of DeepSeek Harness》— infoq.com/news/2026/08/deep-seek-harness
- awesome-harness-engineering — github.com/walkinglabs/awesome-harness-engineering

## 深入浅出

**一句话本质**：按「工程能力 × 约束需求」选层——层越高能力越强但门槛越高，各层服务不同用户，**匹配 > 求高**。

**反模式**：

| 反模式 | 后果 | 正解 |
|--------|------|------|
| 专业者用低层（L0/L1） | 不可控、输出不确定 | 用 L2/L3 要可控 |
| 外行用高层（L2/L3） | 门槛高不会用 | 用 L1 搭 MVP |
| 全员上 L4 | 成本 > 收益 | 按需选层，POC 最小 |

## L0~L4 分层表（能力单调递增）

| 层 | 工具 | 用户画像 | 工程背景 | 交互范式 | 能力边界 | 适用任务 | 示例企业选择 |
|----|------|---------|:---:|---------|---------|---------|:---:|
| **L0 裸聊 chatbot** | ChatGPT 网页 | 尝鲜者/一次问答 | 无 | 纯对话 | 无工程能力（无上下文管理/无约束，知识零散投喂） | 单轮问答 | ✗ |
| **L1 外行低代码** | Lovable/Bolt/n8n/workbuddy | 非程序员/业务用户 | 无 | 描述式生成（describe-and-generate） | 0 代码搭 MVP/流程自动化，输出不确定 | 原型/自动化 | ✗ |
| **L2 中间 IDE** | Cursor/Copilot/Windsurf | 中高级开发者 | Medium | 协作式 collaborator（"你写 AI 补"） | IDE 内多文件编辑、视觉 diff 逐行接受 | 日常开发 | △ 按需 |
| **L3 专业 CLI** | Claude Code/Codex CLI/Gemini CLI | 专业开发/架构师 | High | 命令式 executor | 全仓重构/长自主会话/CI headless、受控精准 | 全仓级重构、CI 自动化 | ✅ 日常 |
| **L4 操作系统级 harness** | Omarchy/dsh/Codex Harness | 顶级工程师/平台团队 | High+ | 环境式，Agent 一等公民 | 最高：自建工具/规则/流程/知识体系（AI 原生 OS） | 自建 harness 体系 | 演进方向 |

## 选层决策

| 维度 | 判断 | 倾向 |
|------|------|------|
| 项目复杂度 | POC → L1 最小；存量 → L3/L4 | L1 → L3 |
| 团队成熟度 | 个人试用不搭重型；平台团队 → L4 | L1→L3→L4 |
| 约束需求 | 生产要「输出可控」 | 避开 L0/L1，用 L3 |
| 自建诉求 | 有 harness 体系诉求 | L4 |

## 检查清单

- [ ] 层级匹配用户工程能力？（外行不推 L3 CLI）
- [ ] 生产允许「输出不确定」吗？（否 → 避开 L0/L1）
- [ ] 有自建 harness 诉求？（有 → L4；日常 → L3）

## 版本历史

| 日期 | 版本 | 变更 | 变更人 |
|------|:---:|------|--------|
| 2026-08-25 | V1.0 | 新建：L0~L4 工具分层（原排序：低代码误置 L3、CLI 误置 L1） | Claude Code（操盘：XingTu） |
| 2026-08-25 | V1.1 | **修正排序**：按能力/掌控度单调递增重排——裸聊 L0 → 低代码 L1 → IDE L2 → CLI L3 → OS harness L4；删除"形态演进轴"打补丁说明；反模式/选层/检查清单同步 | Claude Code（操盘：XingTu） |
