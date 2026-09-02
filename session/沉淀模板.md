# 沉淀模板 — Knowledge Capture / Lessons Learned

> 定义：把「发现/坑/经验」从大脑与会话落成可检索文件，让一次性输出变成组织资产（复利进化）。
> 依据：知识中心化复盘（spike.sh）——知识是集体资产，记"学到什么"非"发生了什么"；复盘研究（Aalto）——事后采集易丢失，须嵌日常。
> 用途：通用知识/发现/坑统一落盘总入口；Bug 走 BUG修复沉淀模板、会话走 会话复盘模板。

## 一句话本质
**每个发现有落点，知识只记一遍。** 使用 → 发现 → 沉淀 → 反哺 → 下次更好。

### 反模式
| 反模式 | 后果 | 正解 |
|--------|------|------|
| 知道了不写 | 知识随会话蒸发，二次踩坑 | 发现即落盘 |
| 到处散落/双写漂移 | 同一事实多份，改一处漏一处 | 单一事实源，先查后写 |

## 沉淀类型 → 落盘位置速查
> 位置规则见 CLAUDE.md 十一节，此处只路由不复制。

| 类型 | 落盘位置 | 登记索引 |
|------|---------|---------|
| Bug/坑 | `incidents/{简述}/` | `memory/bad-case-index.md`（BCI） |
| 会话 | `memory/session-record-{日期}-{主题}.md` | `memory/MEMORY.md` |
| 调研 | `research/{主题}.md` | `research/README.md` |
| 规则 | `rules/{domain}-rules.md` | 规则加载表 |
| 契约 | `contracts/` | `memory/contract-change-log.md` |
| 组件 | `CHANGELOG.md` | CHANGELOG 索引 |
| 单点知识 | `memory/{topic}.md` | `memory/MEMORY.md` |

## 沉淀骨架（5 步）
1. **发现**：教训冒头 → 识别"可复用值得记"
2. **分类**：对照速查表定类型
3. **选落盘**：取目录，新建文件带日期戳 `{名}_{YYYYMMDDHHmmss}.md`
4. **写格式**：套对应模板，只记"为什么/边界/影响"，不记流程编号
5. **登记索引**：更新 MEMORY/README/BCI/CHANGELOG 对应行

## 检查清单（落盘前自问）
```
□ 单一事实源？知识只记一遍，先查已有再写（防双写漂移）
□ 索引可达？文件登记到对应索引了吗
□ 可溯源？结论带来源（文件路径/代码行号/工单号/提交 ID）
□ 反哺了吗？够格升级为红线/Gate/规则吗（错只犯一次）
```

## 引用
- [Knowledge-Centered Postmortems](https://spike.sh/glossary/knowledge-centered-postmortems/) — spike.sh
- [Post-Mortem Analysis as Knowledge Management](https://aaltodoc.aalto.fi/server/api/core/bitstreams/d486b795-fab3-4941-b0c5-86f1cea77e8a/content) — Aalto 研究
