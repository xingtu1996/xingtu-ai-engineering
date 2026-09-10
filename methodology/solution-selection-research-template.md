# 方法模板：方案选型调研（Solution Research / Technology Selection）— 多源对比，选能落地方案

## 一、定义
**方案选型调研**（Solution Research / Technology Selection）——针对技术选型/工具/竞品/趋势，多源聚合（WebSearch 多角度 + WebFetch 深读官方源 + 本地 research/ 查既有 + incidents/specs），产出「结论 + 证据 + 来源 + 决策建议」的结构化调研，不闭门造车。

## 二、深入浅出
**一句话本质**：调研→对比→选型→决策——先查本地有没有，再多源搜外部，最后决策落到 specs。
**反模式**：只看一家就拍板｜不溯源（数字无出处）｜调研完无决策落地（纸面调研）｜重复调研（不查本地 research/）。

## 三、调研文档骨架（对齐 research README 规范：一句话结论/调研发现/决策/来源/关联）
1. **调研背景**：要选什么型、为什么、约束（内网/云依赖/成本）
2. **方案清单**：候选方案逐一列「名称 + GitHub/官方链接 + 核心特性」
3. **对比表**：特性｜成本｜风险｜适用性横向对比（如 Apifox/Postman/Hoppscotch 三列）
4. **选型经验**：关键洞察 + 区分已验证/待验证/推测
5. **决策结论**：推荐 + 理由 + **落地位置**（spec/research/incidents）
6. **来源备案**：每条关键发现带 URL/文件路径；数字（star/价格）必须标注，无来源标"待查"
7. **关联**：research/ 索引链接 + 相关 spec/incidents

## 四、示例企业实践对应
- **先查本地再搜外部**：`research/README.md`（30+ 实例索引，含一句话结论）→ 命中即停，未命中再 WebSearch 2026 成熟方案。
- **多源聚合**：WebSearch 多角度 + WebFetch 深读官方源 + 本地 research/incidents/specs；走 `agents/researcher.md`（多源聚合 + 溯源 + 决策建议）。
- **溯源**：文档落 `research/{YYYYMMDD}-{主题}.md`，结构同上骨架；真实实例 `20260806-tools-ai-mcp-capabilities.md`。
- **决策落地到 specs**：调研终点 = 有地方落实（spec/research/决策），非纸面；如工具调研→EXPLORE spec。

## 五、检查清单
□ 先查本地 research/（防重复调研）？ □ 方案 ≥2 家且多源？
□ 每条发现可溯源（URL/文件路径）？数字有来源？
□ 有对比表（特性/成本/风险/适用性）？ □ 区分已验证/待验证/推测？
□ 决策落哪（spec/research/incidents）？ □ 更新 research/README.md 索引？

**版本历史**：2026-08-25 新建
