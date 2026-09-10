---
name: 反哺模板
description: 坑→BCI→红线→自动拦截，错只犯一次
---

# 反哺模板（Feedback Loop / Standard Work）

> **定义**：把踩坑沉淀为防复发机制——错只犯一次，下次自动拦截。对应 **Standard Work**（标准化作业=改进基线）与 **Feedback Loop**（异常停线+根因分析防再发）。

## 〇、引用备案

| 引用 | 来源 | 对应 |
|------|------|------|
| TPS：Jidoka 异常即停线+根因分析防再发 | [Toyota TPS](https://global.toyota/en/company/vision-and-philosophy/production-system/) | 必须「验证拦截生效」 |
| Poka-Yoke（新乡重夫）：防呆防错误逃逸下一环 | [SixLeanSigma](https://www.sixleansigma.com/lean-process-optimization--andon-system--error-proofing--poka-yoke---defect-vs-errors.html) | Gate 机械生效，不靠自觉 |

## 一、深入浅出

**本质**：坑→BCI→红线→自动拦截，错只犯一次。

**反模式**：
- 记了不拦：BCI 只写不编 Gate（记录≠预防）。
- 机制无落地：红线无 hook 强制（文字≠机械生效）。

## 二、反哺四级流水线

> 参考 `specs-rules.md` §十一，操作化不复制。

| 级 | 动作 | 落点 |
|----|------|------|
| ① 沉淀 | 排查+根因+修复闭环 | `incidents/{工单}-{日期}/` |
| ② 抽象 | 提炼模式：根因→现象→Gate | `bad-case-index.md` BCI-NNN |
| ③ 编码 | 能机械拦截→落红线 | `constitution.md` + rules |
| ④ 拦截 | hook/门禁/审查命中 | hooks / 脚本 / validator |

## 三、反哺骨架

```
发现坑→①incidents→②BCI→③Hard Gate?
  ├能机械拦截→constitution 红线/rules/hook
  └不能→流程清单（validator/对抗审查）
→④验证拦截生效（触发一次确认）→CHANGELOG
```

**BCI 字段表**：

| 字段 | 说明 | 示例 |
|------|------|------|
| ID | BCI-NNN 递增 | BCI-029 |
| 失败症状 | 踩坑现象 | subStock 并发超扣 |
| 文件证据 grep | 复现/定位命令 | `grep "subStock"` |
| 修复模式 | 正确做法=Gate | 防重硬兜底=DB 唯一约束 |

## 四、检查清单

```
□ incidents/ 一工单一目录？□ BCI（根因→现象→Gate）？
□ 机械拦截已判定（Hard Gate/流程清单）？□ 触发验证命中（非纸面）？
□ 只做加法（RED-5 不覆盖已有）？
```

## 五、关联

`harness-philosophy.md` ｜ `specs-rules.md` §十一 ｜ `bad-case-index.md` ｜ `工程实践-loop-engineering.md`
