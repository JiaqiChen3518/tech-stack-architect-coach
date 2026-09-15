# Changelog

All notable changes to this project will be documented in this file.

## v0.1.0 — 2026-09-14

第一个正式版本：评估完成，去除 alpha 状态。

### Added

- `evals/test-report.md` — 完整实测报告：多 Subagent 独立上下文批跑，E1-E11 全量 Trace 与评分。
- Git 标签 `v0.1.0`。

### Changed

- 目录结构迁移至 `skills/tech-stack-architect-coach/`（适配 skills.sh 索引规范）。
- README：状态行与评估章节更新为实测结果（11/11 场景通过、R7 一致性 100%、Skill Lift +275%）。

### Verified

- E1-E5 诊断 + 协商全部通过（多数满分），E6/E7 不触发率 100%，E9-E11 门禁攻防（R3 硬否决项）零穿透。
- R7 模块池跨会话一致性：同输入两次独立生成，相似度 100%。
- 真实教学阶段 R4=4.75/5、R5=4.67/5，产物真实落盘可核验。
- Skill Lift +275%（裸助手基线 4/15 vs 带 Skill 15/15）。

## v0.1.0-alpha — 2026-09-12

### Added

- Initial release of the Tech Stack Architect Coach skill.
- `SKILL.md` — entry protocol with role definition, hard constraints, and stage-gated workflow.
- `references/diagnosis-protocol.md` — three-step diagnosis with scenario-based level assessment.
- `references/module-pool-generation.md` — dynamic module pool generation for any dev tech stack (8-14 modules, 4 layers, with Redis/Kafka/MySQL/Vue examples).
- `references/curriculum-negotiation-protocol.md` — route draft, confirmation gate, and mid-execution adjustments.
- `references/session-management.md` — cold-start five-check, context monitoring with explicit anchors, and three-deliverable wrap-up.
- `references/teaching-preferences.md` — 8 configurable teaching preference items with defaults.
- `references/context-template.md` — CONTEXT.md handoff document template (14 sections).
- `references/trace-template.md` — TRACE.md append-only trace and glossary template.
- `references/examples/` — filled examples for Redis CONTEXT.md, Redis TRACE.md, and Kafka route draft.
- `evals/eval-cases.md` — A/B eval methodology with 8 scored scenarios + Skill Lift metric.
