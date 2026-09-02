# 方法：知识可视化（Knowledge Visualization / README+Dashboard）

> 定义：同一知识双形态——README 文本真相源（Git 可 diff/AI 可读），HTML 看板可视化（统计/筛选/搜索），图比干读高效。
> 引用备案：
> - DEV(2026) Karpathy PKB with Git：Git 仓库即知识库，纯文本可移植、版本原生。 https://dev.to/xunxing_mao_fac71e331fd4b/practicing-karpathys-personal-knowledge-base-method-with-a-git-repository-1o8f
> - DEV(2026) Notion vs Obsidian：Obsidian 双链图谱强，单机协作弱。 https://dev.to/pickuma/notion-vs-obsidian-which-knowledge-base-fits-your-developer-brain-in-2026-f2k

## 深入浅出
- 本质：知识只记一遍（README），视图按需生成（HTML）——文本管真相，看板管扫视。
- 反模式：①双写漂移（两处数据不同步）；②数据硬编码（无单一真相源）。

## README+HTML 看板模式（示例企业 3 实例）
| 实例 | 文本真相源 | 看板 |
|------|-----------|------|
| 模板 | `templates/README.md` | `templates-dashboard.html` |
| Spec | `specs/README.md` | `specs-dashboard.html` |
| Harness | `toolbox/DIRECTORY.md` | `harness-overview.html` |

要点：
- **内联数组真相源**：`const DATA=[...]` 注释「同步自 README」，看板=README 镜像
- **同步**：README 先增行→数组对应增；单文件 HTML 内联 CSS/JS，file:// 可看
- **卡片+筛选+搜索**：聚合卡/chip/实时搜索；语义色反 AI 味

## Obsidian 对比表
| 维度 | README+HTML | Obsidian |
|------|------------|----------|
| 版本化 | Git 原生 | 需 Git 插件 |
| 团队共享 | Git+MR | 单机同步弱 |
| CI 校验 | 脚本可核验 | 无 |
| AI 可读 | 纯文本零噪音 | `[[]]` 需适配 |
| 可视化 | 手写看板 | 内置图谱 |
| 双链导航 | 索引/搜索 | 强 |
| 本地性 | 本地+远端 | 本地 vault |

**结论**：团队+Git+AI 驱动→README+HTML；个人+双链→Obsidian；组合：md 存 Git、Obsidian 当前端（示例企业不引入）。

## 骨架
```
README 源（先增行）→ 同步（脚本优先/手工镜像）→ HTML 看板（内联数组渲染）→ 登记索引（README 头看板行）
```

## 检查清单
- □ 单一真相源？（README 与 HTML 只一处改）
- □ 双写漂移防了？（改数后数组同步）
- □ 单文件可看？（file:// 双击开，零依赖）
- □ 反 AI 味？（系统字体/语义色/无渐变）
- □ 登记了？（README 头部看板行 + 索引行）
