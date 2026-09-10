# 方法-UIUX设计管控（UIUX Governance / Design Token）

> 定义：**UIUX 设计管控** = 生成前端样式/交互前先定设计系统（token + 组件规范 + UX 交互 + 反 AI 味），产出有辨识度、可维护、不撞 AI 味。
> 作用：需求原型 / 前端界面实现 / 代码 Review 的视觉+交互统一基准——UI 视觉层（token/组件样式）+ UX 交互层（弹窗决策/反馈三态/可访问性）。
> 引用备案：Design Token（Salesforce Lightning 2017）；frontend-design；web-design-guidelines。

## 一、深入浅出

**一句话本质**：AI 生成样式前先定设计系统——token 先行，防 AI 味默认蓝紫渐变、无 token 硬编码色、组件各写各的、交互反馈缺失。

**反模式**：无 token 硬编码色｜默认 antd 蓝不覆盖主题｜Modal/Drawer 混用、圆角 4/8/16 混｜蓝紫渐变/三等卡片/Inter 字体（AI 味）｜失败/空态无引导文案。

## 二、Design Token 骨架（引用示例企业金黑）

```css
:root {
  --brand-gold: #CDAE7C;  --brand-black: #221F20;
  --primary: var(--brand-black);  --primary-text: var(--brand-gold);
  --text: rgba(0,0,0,.65);  --text-strong: rgba(0,0,0,.85);
  --border: #d9d9d9;  --border-split: #e8e8e8;
  --success: #52c41a;  --warning: #faad14;  --error: #ff4d4f;
  --font: 'Microsoft YaHei', 'PingFang SC', sans-serif;
  --radius: 4px;  --shadow-modal: 0 4px 12px rgba(0,0,0,.447);
  --control-h: 32px;  --grid: 8px;
}
```

## 三、组件规范清单

| 组件 | 示例企业标准 |
|------|---------|
| 操作弹窗 | **居中 Modal**（宽 520–1200），禁 Drawer；标题 16px/500；footer [取消][确定(金黑)] |
| 组件库 | antd 3.x（实装 3.26.15），variables.less 覆盖主题，非默认蓝 |
| 色值 | 全走 Less 变量（variables.less），禁散落硬编码 |
| 主按钮 | 金字 #CDAE7C + 曜黑底 #221F20，hover #2E2B2C |
| 表格 | 表头 #fafafa、行线 #e8e8e8、hover #e6f7ff、激活分页金色 |
| 字体 | Microsoft YaHei / PingFang SC，不用外链字体 |

## 四、UX 交互与可用性

| 场景 | 规则 |
|------|------|
| 反馈三态 | 成功/失败/空态给方向：报错说清+修复，空态邀行动 |
| 文案 | 动词化，全流程同名，用户视角命名 |
| 可访问性 | 键盘焦点、reduced motion、响应式 |
| 操作决策 | 短任务=Modal；长表单/多步骤/详情才跳页或 Drawer |

## 五、反 AI 味检查（frontend-design）

□ 先定审美（品牌金黑），不套模板默认 □ 禁蓝紫渐变/三等卡片/Inter 字体
□ 主色非 #1890ff，激活态金色 □ 圆角 4px（非 8/16 AI 味）

## 六、示例企业实践对应

| 实践场景 | 做法 | 依据 |
|----------|------|------|
| 需求原型/界面实现 | 先定 token（金黑）再写 UI；弹窗居中 Modal；全色值走 Less 变量 | `research/20260807-panshi-ui-baseline.md` |
| 审美方向 | 先定品牌金黑，不套模板默认，禁 AI 味 | `skills/frontend-design` |
| UI 代码审计 | 对照 Web 标准输出 file:line 问题 | `skills/web-design-guidelines` |
| 代码 Review | 过 §七 检查清单（token/组件/反 AI 味/UX） | 本模板 |
| 实证校验 | ctf-backend：Modal 268/Drawer 1（配置抽屉例外）；antd 实装 3.26.15；主题变量在 variables.less | 2026-08-26 实测 |

## 七、检查清单

□ token？（无硬编码色） □ 组件？（Modal 非 Drawer / antd 3.x / 金黑） □ 无 AI 味？ □ 全 Less 变量？
□ 反馈三态+文案动词化？ □ 可访问性？

**版本历史**：2026-08-26 新建（UI 样式管控 → UIUX 设计管控融合版）
