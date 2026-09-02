# XingTu AI Engineering

> **One repo, the full landscape of AI engineering**: philosophy · methodology · five engineering practices · governance · session resume · scaffolds

![MIT](https://img.shields.io/badge/license-MIT-green.svg)

---

## What is this

`xingtu-ai-engineering` is the flagship methodology repository of the XingTu open-source matrix. It distills years of hands-on AI engineering practice into structured, **copy-paste-ready templates** (with checklists, commands, and verification steps) across six categories.

**Audience**: tech leads, engineers, and AI engineering practitioners who want a systematic understanding of how to harness AI for productive output — not scattered tips.

## The Five Engineering Practices (core)

| Engineering | File | What |
|---|---|---|
| **Context Engineering** | `engineering/上下文工程.md` | Planning, compressing, and budgeting context window/tokens |
| **Prompt Engineering** | `engineering/提示词工程.md` | Structured prompt design & reuse |
| **Harness Engineering** | `engineering/驾驭工程.md` | Rules/skills/hooks that turn models into stable productivity |
| **Graph Engineering** | `engineering/图工程.md` | Dependency/knowledge/task graphs for planable, traceable systems |
| **Loop Engineering** | `engineering/循环工程.md` | Feedback → fix → persist loops for continuously evolving output |

## Six Categories · 73 assets

- `philosophy/` (11) — ontology, first principles, compounding evolution, shift-left testing, truth first, adversarial collaboration, ...
- `methodology/` (37) — SDD, TDD/BDD/DSL, adversarial review, progressive disclosure, requirement clarification, impact analysis, Git worktree, Harness hierarchy, test templates (black/white/mutation/load/API/UI/E2E/compile), ticket reply, incident troubleshooting, delivery checklist, quality gates, ...
- `engineering/` (9) — context/prompt/harness/graph/loop engineering + TDD + automation ontology + Jenkins CI
- `governance/` (1) — constitution template (S/M/L tailoring, quality gates, delivery baseline)
- `session/` (6) — session resume prompt, session review, bug-sediment, feedback, ponytail/caveman share
- `scaffolds/` (9) — agent template, MCP scaffold, code-package template, service structure, Mermaid scaffold, MR template, SOP template, story template

## AI discoverability

`marketplace.json` provides a machine-readable index (name + description + path + tags) for find-skills / AI search engines:

```bash
cat marketplace.json   # 73 asset entries
```

## License

MIT License.

---

> AI-assisted creation · Built on real engineering practice · All company-sensitive information removed
