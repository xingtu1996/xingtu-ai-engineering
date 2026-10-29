# AGENTS.md — xingtu-ai-engineering

## Project-specific instructions
Read `README.md` and any directory-level agent instructions before edits. This flagship methodology repository aggregates engineering practices. Keep method descriptions, example code, and governance claims aligned; do not silently replace project-specific implementations with generic templates.

## Shared assets and scope
- Shared Skills: https://github.com/xingtu1996/xingtu-skills
- SDD engine: https://github.com/xingtu1996/xingtu-sdd
- Reusable rules and hooks: xingtu-rules and xingtu-hooks.
These are references, not automatically inherited rules or installed tools. Verify supported installation paths and pinned versions before invoking them.

## Delivery and evidence
1. Inspect source of truth, repository-specific constraints and actual tests.
2. Write acceptance criteria and an SDD spec before nontrivial implementation.
3. Use isolated branches and reviewable Draft PRs; do not merge, publish, change license or visibility without approval.
4. Run safe checks and record actual commands, CI links and results; mark unrun checks NOT_RUN.
5. Never disclose private Vault material, secrets, personal data or private customer content. Preserve a rollback commit.
