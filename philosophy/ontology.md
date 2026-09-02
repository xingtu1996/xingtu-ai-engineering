# 理念-本体论（Ontology）

> 定义：本体论=领域**概念/实体/关系的显式规范**（类/属性/关系/约束），是图的**语义层**——无本体图只是数据，有本体图才有可推理语义。
>
> **渊源卡**：
> - 哲学：源亚里士多德"存在"研究（metaphysics）。
> - 工程：**Gruber 1993**《A translation approach to portable ontology specifications》首提「本体=概念化的显式规范（an explicit specification of a conceptualization）」；Studer 1998 补「形式化的共享概念化规范」。
> - 语义网：RDF 三元组→RDFS 轻量词汇→OWL（2004 描述逻辑形式语义）→SHACL（闭世界校验）。

## 背书/引用备案（背书双轨）

**外部权威**：

| 引用 | 作者/年 | 一句话理念 | URL |
|------|--------|-----------|-----|
| A translation approach to portable ontology specifications | Gruber 1993 | 本体=概念化的显式规范，含类/关系/公理/实例（渊源卡主源） | https://doi.org/10.1006/knac.1993.1008 |
| Knowledge Engineering: Principles and Methods | Studer, Benjamins & Fensel 1998 | 补「形式化的共享概念化规范」（formal + shared） | https://doi.org/10.1016/S0169-023X(97)00056-6 |
| OWL 2 Overview | W3C 2004/2012 | 描述逻辑形式语义，机器可解释 | https://www.w3.org/TR/owl2-overview/ |
| Ontology Development 101 | Noy & McGuinness 2000 | 先 scope+能力问题→优先复用→定义类/属性/约束 | https://protege.stanford.edu/publications/ontology_development/ontology101.pdf |
| Palantir Foundry Ontology Core Concepts | Palantir 2020s | 本体=企业数字孪生：语义层（对象/属性/链接）+动力学层（Action 受控写回）+动态层（仿真/AI 决策/决策捕获），是决策与行动层 | https://www.palantir.com/docs/foundry/ontology/core-concepts |
| Ontology: A Structure for Decisions, Not Data | Tiro Blog | "Nouns must be paired with verbs"；本体为决策而生非数据；可审计决策传播=分析与操作系统分界线 | https://tiro.ooo/en/blog/ontology-structure-for-decisions |
| palantir-ontology-strategy（渐进吸收） | Leading-AI-IO | progressive absorption：read-first 挂接→逐系统引入 Actions→退役遗留段；本体停止触碰即腐烂 | https://github.com/Leading-AI-IO/palantir-ontology-strategy/blob/main/README_en.md |

**内部实证（示例企业落地）**：

- `.claude/contracts/schema/{名}.schema.json` — 微服务边界微型本体实证（required/类型/enum/additionalProperties:false 即类/属性/约束），见 `方法-dsl-template.md` §三
- CBM 节点/边类型 = 代码世界本体（`工程实践-graph-engineering.md`）— search_graph/trace_path 即语义检索实证
- `.claude/rules-on-demand/specs-rules.md` §5.4 DSL 硬化 — 契约 Schema + Gherkin 把本体约束变机器强制层（漂移即红）

## 深入浅出

**一句话本质**：无本体图只是数据，有本体图才有可推理语义——概念/实体/关系显式定义，机器才能解释/校验/推理。

**反模式**：无 schema 自由文本（语义靠人脑）；概念歧义（同字段跨服务漂移）；过度建模（先搜已有再建）。

## 示例企业实践对应（引用不复制）

- **契约 Schema**：`contracts/schema/{名}.schema.json`（required/类型/enum/additionalProperties:false）= 微服务边界的微型本体。见 `方法-dsl-template.md` §三。
- **CBM 元数据模型**：CBM 节点/边类型 = 代码世界本体，search_graph/trace_path 即语义检索。见 `工程实践-graph-engineering.md`。
- **DSL 硬化**：契约 Schema + Gherkin 把本体约束变机器强制层。见 `specs-rules.md` §5.4。

## harness 组件本体论（OOP/OOA 类比）

> harness 自身就是一套本体：**组件=抽象封装，注册器=显式实例化，渐进式加载=按需实例化**。像 OOP 的类→对象，harness 的"概念显式化"落到可检索结构。

| OOP/OOA 概念 | harness 对应 | 落地位置 |
|-------------|-------------|---------|
| 类/抽象 | 组件类型（skill/agent/rule/script/hook/contract/workflow） | 各组件目录 `_TEMPLATE` |
| 属性/方法 | 组件骨架（frontmatter/描述/触发词/执行产出） | 组件文件本身 |
| 封装（私有） | 组件实现细节不外露，只暴露 description 触发 | skill/agent 文件 |
| 实例化 | 注册器登记（描述+触发词→可路由） | `DIRECTORY.md` / `toolbox/` / `CHANGELOG.md` |
| 继承/复用 | 样板 _TEMPLATE 复制套用 + harness-gate 准入 | `templates/` 五类样板 |
| 接口规约 | 全局规约（constitution/CLAUDE.md/rules）约束所有组件 | `rules/` + `CLAUDE.md` |
| 延迟实例化 | **渐进式加载**：常驻指针 → 触发路由 → 按需取全文 → 释放 | 渐进式披露表 + Skill 路由 + `方法-progressive-loading.md` |
| 单一职责 | 一组件一事，可测试可回归 | harness-gate 准入五问 |

**每个组件标准三件套**（用户确立惯例）：① 抽象文件夹（组件本体）② README/自述文件（说明书）③ 注册器登记（CHANGELOG/DIRECTORY/toolbox 全局可见）。

## AI 时代层：从数据到行动（Palantir Ontology 借鉴）

> **一句话**：传统中台告诉你"库存不够了"（语言层/昂贵镜像），本体论帮你"把单下了"（执行层/感知→决策→写回→闭环）。"Nouns must be paired with verbs"——数据库存名词事实，决策不是事实列表。

### 三层本体模型（Palantir Foundry 官方）

| 层 | 要素 | 示例企业对应 |
|----|------|---------|
| 语义层 Semantic | 对象类型/属性/链接（行→对象、列→属性、join→链接），数字孪生 digital twin | contracts/ + CBM 对象图谱 + 对象别名表 |
| 动力学层 Kinetic | Action 类型（受控写回，side-effect：通知/webhook/外部调用）+ Function；"从看到到行动的 watershed 分水岭" | 自动化脚本（jenkins-ci-build/rancher redeploy/MQ 消费者/Zeebe worker） |
| 动态层 Dynamic（harness 落地称决策层） | 仿真、AI 决策、决策捕获与学习（结果回喂模型，越用越聪明） | incidents/ + BCI + bad-case-index 决策回喂 |
| （之下）基础设施 | pipeline/数据集成不在三层内 | rtk/CBM/数据库连接 |

**关键定位**：Ontology 是"决策与行动层"非纯语义层；可审计的决策传播是分析系统与操作系统的分界线。官方引文："The Ontology is designed to represent the complex, interconnected decisions of an enterprise, not simply the data"；"Schemas describe data; ontologies describe reality"。

### 案例（背书双轨）

- **BP**：200 万传感器数字孪生，决策时间数天→数小时；累计约 10 亿美元成本优化（注：10 亿/72h/380 万为聚合站转述，官方稿未含）；Mad Dog 预测性维护提前 72h/92% 准确率省约 380 万美元；2024-09-09 五年战略协议 + AIP（LLM 决策建议 + data provenance 防幻觉 + 完全可审计）。
- **Airbus**：Foundry 整合工程/生产/质控/供应链，A350 交付提速约 33%、一年产量翻四倍（"25x"仅单一中文来源非官方 ROI 口径，引用需标注）。
- **Palantir×xAI×TWG**（2025-05）：Grok + Colossus，目标企业 CEO 部署数十万 AI agent，TWG 主导实施。
- **渐进落地**：progressive absorption——先 read-first 只读监控层挂接遗留系统，验证一个闭环后逐系统引入 Actions 写回并退役遗留段，反对 big-bang。"本体在你停止触碰它的那一刻开始腐烂"。

### 中台 vs 本体论

- 数据中台"已死"动因：数据消费主体从人转向 AI，95% 数据平台操作将由机器接管，"给人看"的 BI 宽表/看板场景萎缩，引擎类组件保留。
- 纯看板批判："你构建的只是一个昂贵镜像——观察现实但不操作现实，那是 dashboard，不是闭环"。
- 推荐顺序：数据平台→BI/看板→知识库→Agent——"数据平台是大脑皮层，Agent 是嘴巴和手脚"，跳过基础直接上 Agent 是让手脚没有大脑皮层。

### AI 产品经理四能力（映射示例企业）

| 能力 | 业界 | 示例企业落地 |
|------|------|---------|
| 业务语义翻译 | 业务痛点↔AI 需求规格双向翻译 | specs analysis/requirements 前置（业务术语字典） |
| 对象建模 | 定义 Agent 边界/触发条件/人工介入 | CBM 对象图谱 + contracts 归一 |
| 评估体系 | LLM-as-Judge，把"感觉变好了"变"可验证地变好了" | validator + 任务成功率/步骤稳定性/异常恢复率 |
| 幻觉管控 | 数据溯源 + 审计 + 边界 | constitution 禁止模式 + security-rules 审计 |

> 据报道 LinkedIn 2025 AI PM 岗位 +340%、2026 春招 +369%（今日头条转述，待官方验证）；PM:工程师比 1:5→1:2（无直接来源）。机会属于懂业务语义+对象建模的复合型人才。

### 示例企业落地：名词+动词双要素

- **名词**=对象/属性/关系（contracts + CBM）；**动词**=可执行写回（Feign/HTTP/MQ/Zeebe worker）。
- **轻量 action-registry**：登记自动化脚本的输入/校验/副作用/审计，先以 harness 自身（build.sh/rancher-tool/new-branch.sh）试点，不必引入完整本体建模产品。
- **Decision Capture**：把工单决策链（数据→逻辑→行动→结果）落盘为可回放记录，强化复利进化 loop 机械闭环（人审保留=HITL）。
- **受控 Action 出口**：AIP agent 只能通过预定义 Action 行动（RBAC + 高风险人审 + 审计）；示例企业对应"AI 只通过 Action 行动，禁止裸调 DB/SQL/Feign"——承接 constitution 禁止模式（事务内远程调用/消费者不幂等）作 Action 级校验规则。

## 备注卡（速查）

- **关系**：本体（概念/关系定义）→Schema（结构实现）→契约（跨服务边界落地）→DSL（机器可校验，漂移即红）。DSL 化只对需机器校验的边界。
- **何时建本体**：数据/图跨 2+ 服务共享且语义易歧义（共享表字段/Feign DTO/MQ 事件体/状态机）；单机内部不建。
- **harness 本体**：组件抽象 + 自述 + 注册器 + 全局规约 + 渐进式加载 = 可检索可路由可维护。

**版本历史**：2026-08-25 新建；2026-08-25 V1.1 补 harness 组件本体论（OOP/OOA 类比 + 注册器 + 渐进式加载）；2026-08-27 V1.2 补 AI 时代层（Palantir 三层模型/中台对比/案例/AI PM 四能力/示例企业名词+动词落地），调研来源 spec `2026082711-ontology-ai-era-enterprise`
