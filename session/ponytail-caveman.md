# Ponytail & Caveman — 开源 AI 编码精简工具（分享介绍）

> 用途：分享给团队/他人。两个开源 skill 解决「AI 过度工程 + 冗长输出」。示例企业 harness 已复用验证。

---

## 一句话

**ponytail**（YAGNI 最少代码）+ **caveman**（散文压缩）——让 AI 只写需要的代码、只说必要的话。实测 **代码 -54%、输出减 30-50%、100% 安全保持**。

## 解决什么问题

```
❌ 痛点：AI 做"番茄炒蛋"加了"东坡肉"（不需要的无关功能），PR 注释还解释一大堆"为什么不需要"。
✅ ponytail：编码前问"需要吗？不需要→不写"（YAGNI 决策阶梯）
✅ caveman：输出/注释精简（去客套/去废话）
```

## 适配（跨 agent）

| Agent | 接入方式 |
|-------|---------|
| **Claude Code** | `/plugin marketplace add DietrichGebert/ponytail` → `/plugin install`；caveman 同理 |
| **Codex** | `npx skills add DietrichGebert/ponytail` |
| **Cursor / Windsurf / Copilot / Gemini CLI / Cline** 等 | 40+ agent 兼容（`npx skills add`）|

## GitHub + Star + License

| 工具 | GitHub | Star | License |
|------|--------|:---:|:---:|
| ponytail | [github.com/DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) | **111,359** | MIT |
| caveman | [github.com/JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) | **100,986** | MIT（Engine 部分 BSL-1.1）|

## 用法

### ponytail（生成层：最少代码）
```
/ponytail            全量（默认）
/ponytail lite/ultra 强度调节
/ponytail-review     过度工程扫描（PR 前用）
/ponytail-audit/debt/gain  审计/债务/收益
```
核心：**YAGNI 决策阶梯**（写代码前逐级问）：
`不需要就不写 → 已存在就复用 → stdlib → native → 已装依赖 → 一行 → 最少可用代码`
> 铁律：验证/错误处理/安全/无障碍**从不精简**，只砍纯实现细节。

### caveman（散文层：精简输出）
```
/caveman             全量（默认）
/caveman lite/full/ultra 强度
/caveman-stats       节省统计
```
> 只减输出 token；代码/路径/版本号/安全警告**从不压缩**；破坏性操作自动退出精简。

## 实测数据（独立基准）

| 指标 | ponytail | caveman |
|------|:---:|:---:|
| 代码量 | **-54%**（404→23 行日期选择器）| — |
| Token | -22% | 输出 **-30~50%** |
| 成本/时间 | -20% / -27% | — |
| 安全 | **100% 保持** | 技术准确保留 |

> ⚠️ 诚实：caveman 宣传 75% 是基线虚高，独立基准 30-50%；每轮加 ~1-1.5K 输入 token。

## 安装

```bash
# Claude Code
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail
/plugin marketplace add JuliusBrussee/caveman
/plugin install caveman

# Codex / 其他 agent
npx skills add DietrichGebert/ponytail
npx skills add JuliusBrussee/caveman
```

## 示例企业实践（验证）

- 已在示例企业 harness 复用（SessionStart 自动激活）
- 六层定位：ponytail=生成层、caveman=散文层
- 与 `outputStyle: Concise`（系统级简洁）互补

---

> 关联：`toolbox/04-Skill工具.md`（完整 skill 清单 + 引用链）｜ `rules/harness-philosophy.md`（简单即真元哲学）
