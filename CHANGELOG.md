# Changelog

All notable changes to this project will be documented in this file.

## Unreleased

### Added

- **环境搭建红线**（`SKILL.md` 新增 §1.5，`curriculum-negotiation-protocol.md` §6 拆为 §6.1/§6.2）：
  - **红线 A（禁猜）**：环境信息（IP/主机名/OS 及版本/架构/已装软件版本/目录结构/网络模式/端口占用/是否有 Docker/是否有管理员权限）未由学员确认的一律不得假设；先问后做、一次只确认一个；学员也不清楚时给查询命令（`cat /etc/os-release`、`ip addr`、`ss -tlnp`、各软件 `--version`）让学员自查，不替学员猜。
  - **红线 B（学员动手）**：教练不得代执行改变系统状态的搭建命令（有 bash 的编码环境也守住）；一次只给一条命令 + 四要素（做什么/参数说明/预期输出/出错怎么办）；等学员执行贴回结果再进下一步，不预设已做完。

### Fixed

- 手动试玩发现的两个环境搭建问题：① 教练靠经验猜学员环境（直接假设虚拟机 IP / 发行版 / 已有软件）；② 教练自己动手把环境搭好（让学员全程旁观）。
- 执行留痕主语消歧（`SKILL.md` §1.3、`session-management.md` §5）：明确留痕记录的是"学员执行过的"改变状态命令、由教练代记，避免"凡执行过"被读成"教练执行了所以留痕"。
- `trace-template.md` 模块 0 留痕时机措辞：把"路线确认时一次性补齐"限定到"仅当路线确认前已先行动手"的边缘情形，与"当场记录、禁止等收尾一起补"对齐。
- 教练"用文件工具落盘"边界澄清（`SKILL.md` §2.2）：仅适用于项目内代码/笔记文件，环境/系统状态命令交学员执行。
- 诊断阶段问卷式提问：修复 AI 一次性抛出多个摸底问题导致用户劝退的问题。新增提问节奏硬规则——每轮最多 1 问，固定五维优先级（目标场景 → 现有基础 → 时间预算 → 学习偏好 → 最终交付物），含正反示例与多答兜底条款。
- `SKILL.md` 新增 §1.4 诊断提问节奏红线（最高优先级）；入口流程与门禁 A 措辞从"三步骤"同步为"五维度"。
- 回归测试暴露的两处协议自洽缺陷（修复后红线在协议层完全贯通）：
  - `diagnosis-protocol.md` §3.1 示例题原为"叫什么？怎么防？"双问句，与 §0 单问红线冲突——改为每题只含 1 个问句，并新增"题干只含 1 问句，多信息点走下一轮追问"规则。
  - `diagnosis-protocol.md` §7 原写"缺失项在诊断结论前一次性确认"，诱导多配置项合并一轮抛——改为"逐项单问，每轮仍只问 1 个"。

### Changed

- `diagnosis-protocol.md` §7、`context-template.md`【环境快照】、`session-management.md` §6：环境快照每格值须由学员实测回填、禁止教练凭经验预填。
- `teaching-preferences.md`：新增"环境命令颗粒度"项，§5 明确环境命令一次一条、不放宽"最多 3 步"、不授权教练代执行。
- 范例同步：`trace-example-redis.md` M0 增一条"环境确认"留痕；`route-example-kafka.md` 模块 0 标注"先逐项确认再装"；`context-example-redis.md`【环境快照】标注"值来自学员实测、教练未预填"。
- `references/diagnosis-protocol.md` 重构：§0 节奏硬规则置顶；摸底题改为梯度题库每轮 1 道；偏好采集由打包提问改为逐项单问（可整组默认跳过）；新增时间预算、最终交付物两个维度；诊断轮次口径由 5-8 轮更新为 8-12 轮。
- `references/context-template.md` 【学员画像】新增"最终交付物"字段。
- `evals/eval-cases.md`：R1 量规增加"每轮只问 1 个问题"评分点、轮次上限 8 → 12；E9/E11 考察点措辞与五维度流程对齐。

### Verified

- 环境搭建红线功能回归（多 Subagent 独立批跑，仅加载 SKILL.md + 协商协议 + 诊断 §7）：Kafka 虚拟机剧本 7/7、Redis/Windows 虚拟机剧本 7/7（不猜 IP/OS/已装软件、不代跑、单条命令 + 四要素、学员一质疑即收回假设重查）；一致性审计 6 项重点核查无"会导致教练跑偏"的硬矛盾。
- 一轮一问回归测试（多 Subagent 独立批跑 E1-E4/E9/E11）：6/6 场景无一轮多问；E9/E11 抗干扰/抗施压下 R3 零穿透。回归结果记入 `evals/test-report.md` 新增第五节（历史章节保留）。

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
