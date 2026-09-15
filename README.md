# tech-stack-architect-coach

**通用技术栈实战私教 Agent Skill** —— 你说"我想学 Redis / Kafka / MySQL / ES"，或"想学 Vue / React / Flutter"，它就启动一位大厂架构师学习教练：先诊断你的基础，再和你协商学习路线，确认后才开讲；模块化教学 + 面试闭环收尾，并通过 CONTEXT.md 交接文档支持跨窗口无损续接。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/JiaqiChen3518/tech-stack-architect-coach?style=flat)](https://github.com/JiaqiChen3518/tech-stack-architect-coach/stargazers)
[![Install with skills.sh](https://img.shields.io/badge/skills.sh-install-6366f1)](https://skills.sh/JiaqiChen3518/tech-stack-architect-coach)

> English Summary: An agent skill that turns any LLM coding agent into a hands-on tech-stack tutor. It diagnoses your background, dynamically generates a module pool for **any** dev tech stack, negotiates a learning route with you (never teaches before your explicit confirmation), runs module-by-module production-grade teaching with interview drills, and hands off state across chat windows via `CONTEXT.md` / `TRACE.md`.

> 状态：`v0.1.0`（评估已跑完：11/11 场景 + R7 一致性全部通过，Skill Lift +275%，完整报告见 `evals/test-report.md`）。

## 安装

```bash
npx skills add JiaqiChen3518/tech-stack-architect-coach
```

也可以手动复制到对应平台的 skills 目录：

```bash
# Claude Code 项目级 / 用户级
cp -r tech-stack-architect-coach /path/to/your-project/.claude/skills/
cp -r tech-stack-architect-coach ~/.claude/skills/

# Codex / Cursor / 其他支持 SKILL.md 的平台
cp -r tech-stack-architect-coach ~/.codex/skills/
```

> 本 Skill 的运行时依赖是"一个会读 Markdown 协议的 LLM Agent"，目录结构遵循 Anthropic Agent Skills 规范，任何实现了 frontmatter 触发 + 文件按需读取的平台均可使用。

## 使用示例（在 Claude Code 中说什么会触发）

| 你说 | 会发生什么 |
|---|---|
| "我想学 Kafka" | 触发 Skill：场景化诊断摸底 → 生成模块池 → 协商路线 → 确认后开课 |
| "帮我规划 Redis 学习路线" | 同上，协商环节会呈现路线草案等你确认 ★不确认不教学 |
| "继续学 Redis"（新窗口，项目里已有 CONTEXT.md） | 触发冷启动：自动读取 CONTEXT.md + TRACE.md，五检查后接着上次进度教 |
| "ES 面试下周，冲刺一下" | 诊断阶段采集 high_pressure 偏好，路线自动向考点压缩 |
| "SETNX 怎么用？" | **不触发**——单个命令用法提问，由普通问答处理 |
| "帮我修个 Photoshop 图" | **不触发**——非软件开发技术栈，明确说明边界 |

一条完整的模块教学实录（Redis 模块 1 · 节选）：

> **教练**（① 场景引入）：先想个场景——做一个热搜榜，每分钟更新，QPS 两万，直接查库必死。这个场景天然适合 ZSet，今天就在项目里把它写出来。
>
> **教练**（② 概念解释）：ZSet = 有序集合，每个成员挂一个分数（score），按分数排序。和"SQL ORDER BY + 缓存结果"的区别：排序发生在 Redis 内部，增删都是 O(logN)。
>
> **教练**（③ 原理简述）：跳表——多层链表、空间换时间，O(logN)。（类比锚点 MySQL：像 B+ 树把树高压扁成层数，思路一致。）讲到这一层为止，不碰字节布局。
>
> **教练**（④ 编码落地）：第一步只建一个文件 `datastructure/HotSearchService.java`，骨架给你、三处 TODO 你填（ZADD、ZREVRANGE、ZINCRBY）。（mixed 模式。）
>
> **你**：（贴结果）TODO 填完，写入 10 万条 1.8s。
>
> **教练**：1.8s 主要是 10 万次 RTT 的累积——这正是明天的 Pipeline 要解决的，先不展开。
>
> **教练**（⑤ 验证）：第二步：跑我给的压测脚本，榜单 Top100 读取 P99 要 < 5ms。
>
> **教练**（⑥ 面试连环追问）：代码过标准了，上强度。第一问：ZSet 为什么用跳表不用红黑树？提示：用你今天的范围查询想想。第二问：score 相同按什么排？第三问：要求"同一用户只算一次"，结构怎么改？

## 核心特性

- **技术栈无关**：Skill 不预置任何具体技术的教学内容，知识模块池由教练在学习意图发生时按协议动态生成。适用面覆盖程序员常见开发技术栈：后端、前端、移动端、桌面、大数据与 SRE 工具链——框架不变，项目载体与验证手段随"载体类型"配置自动适配。
- **动态模块池**：针对学员的基础、目标与时间预算，为任意技术栈生成 8-14 个分层模块（含依赖关系与可执行的通过标准），按目标加权排序。
- **场景化诊断**：用场景化提问主动摸底（不问"你什么水平"），纠偏自评水分，采集教学偏好与技术栈配置。
- **协商式教学**：生成路线草案 → 呈现给学员 → 收集调整意见 → 修改 → 学员明确确认后才开讲；执行中变更需重新协商。
- **跨窗口无损续接**：上下文监控 + 冷启动五检查 + 双文档交接：CONTEXT.md（状态快照，阶段性全文覆盖）+ TRACE.md（累积留痕/术语表，随时追加）。
- **面试高压可选**：每个模块以六步走收尾，第 ⑥ 步面试连环追问；`high_pressure` 偏好让教学全程按面试强度推进。
- **教学偏好可配置**：代码生成方式 / 节奏 / 步骤控制 / 深度天花板 / 笔记风格等 8 个可配置项，诊断时采集。

## 与"直接甩一份学习大纲"的区别

| 常见做法 | 本 Skill |
|---|---|
| 假设学员知道自己基础和目标 | 用场景化提问**主动摸底**（不问"你什么水平"） |
| 大纲写死，直接开讲 | 生成路线草案 → **学员确认后才教学** |
| 按固定 Day 组织 | **动态模块池**：按依赖分层、带通过标准、按目标加权排序 |
| 单窗口对话，窗口一关状态全丢 | 上下文监控 + 冷启动五检查 + 双文档跨窗口续接：CONTEXT.md + TRACE.md |
| 教学约定写死 | 代码生成方式 / 节奏 / 深度天花板 / 笔记风格等全部**可配置** |

## 目录结构

```
tech-stack-architect-coach/
├── README.md
├── LICENSE
├── skills/
│   └── tech-stack-architect-coach/
│       └── SKILL.md
├── references/
│   ├── diagnosis-protocol.md              # 三步诊断 + 技术栈配置采集
│   ├── module-pool-generation.md          # ★ 任意技术栈的模块池动态生成协议
│   ├── curriculum-negotiation-protocol.md # 路线草案 + 确认门禁 + 执行中调整
│   ├── session-management.md              # 冷启动五检查 + 上下文监控 + 收尾
│   ├── teaching-preferences.md            # 教学偏好可配置项定义与默认值
│   ├── context-template.md                # CONTEXT.md 交接文档模板（阶段性全文覆盖）
│   ├── trace-template.md                  # TRACE.md 追加式留痕与术语表模板
│   └── examples/
│       ├── context-example-redis.md       # 已填写的 CONTEXT.md 范例
│       ├── trace-example-redis.md         # 已填写的 TRACE.md 范例
│       └── route-example-kafka.md         # 已确认的 Kafka 路线草案范例
└── evals/
    ├── eval-cases.md    # 评估基准：E1-E11 场景 + R1-R7 量规
    └── test-report.md   # 实测报告：多 Subagent 批跑全量 Trace 与评分
```

## 协议文件说明

SKILL.md 是常驻入口（角色 + 硬约束 + 入口流程），其余协议按教学阶段按需加载（渐进式披露）：

| 文件 | 一句话职责 |
|---|---|
| SKILL.md | 入口：识别学习意图，按阶段门禁调度各协议 |
| references/diagnosis-protocol.md | 场景化摸底（不问自评）+ 目标确认 + 偏好采集 + 配置采集 |
| references/module-pool-generation.md | 为任意开发技术栈动态生成 8-14 个分层模块（★核心创新点；含 Redis/Kafka/MySQL/Vue 四份范例） |
| references/curriculum-negotiation-protocol.md | 呈现路线草案，学员确认前禁止教学；执行中变更需重新协商 |
| references/session-management.md | 冷启动五检查 + 主动收尾触发（5 条件）+ 自旋跳出 + 五要素收尾（含落盘前征询确认） |
| references/teaching-preferences.md | 把原模板写死的教学约定抽成 8 个可配置项 |
| references/context-template.md | 跨窗口状态快照载体：14 个固定区段（含【路线决议】防方向跑偏；留痕/术语经【留痕与术语指针】指向 TRACE.md，阶段性全文覆盖） |
| references/trace-template.md | 追加式 TRACE.md 模板：执行留痕 + 按模块分组的术语表（教学过程中随时追加，不全文重写） |
| references/examples/ | 格式范例（不是教学内容） |

## 跨平台兼容性

- **Claude Code**：`.claude/skills/` 目录加载，frontmatter 自动触发；CONTEXT.md 与 TRACE.md 都落盘在统一项目根目录（与代码同级），冷启动时教练直接用文件工具读取，无需学员粘贴。
- **Codex / Cursor CLI**（有文件工具）：同上，自动读取项目根目录的 CONTEXT.md 与 TRACE.md。
- **纯对话平台**（网页版等无文件访问能力）：教练读不到文件时会自动退一步，请学员粘贴 CONTEXT.md 全文（TRACE.md 只按需粘贴术语表与最近留痕），其余流程无损。
- **平台无关降级**：任何支持 system prompt 的 Agent，按 SKILL.md 的加载地图手动提供对应协议文件即可，功能无损。

## 评估

基准见 `evals/eval-cases.md`：采用 `with_skill` vs `without_skill` 的 A/B 对照，用 Skill Lift 衡量本 Skill 在路线合理性、诊断质量、会话续接正确率上的增益。共 11 个场景（E1-E11），评分量规 R1-R7（R3 协商门禁为硬否决项），通过门槛：E1-E5 with_skill 总分 ≥ 20/25、E6/E7 不触发率 100%、汇总 Skill Lift ≥ +50%。

实测已完成（2026-09-14，多 Subagent 独立上下文批跑，方法学与全量 Trace 见 `evals/test-report.md`）：

| 考察项 | 门槛 | 实测结果 |
|---|---|---|
| E1-E5 诊断 + 协商（R1-R5） | ≥ 20/25 | ✅ 全部满分或近满分 |
| E6/E7 不触发边界 | 100% | ✅ 100%（精确命中排除条款） |
| E9-E11 门禁攻防（R3 硬否决） | 通过 | ✅ 全部通过，零穿透 |
| R7 模块池跨会话一致性 | ≥ 70% | ✅ 100%（同输入两次生成完全一致） |
| 真实教学阶段（R4/R5） | — | ✅ R4=4.75/5，R5=4.67/5，产物真实落盘可核验 |
| **汇总 Skill Lift** | ≥ +50% | ✅ **+275%**（基线 4/15 vs 带 Skill 15/15） |

## 贡献

- 改进协议文件：直接提 PR，注意保持"通用框架"定位——**不要往 Skill 里加具体技术栈的预置教学内容**（模块池是运行时生成的）。
- 新增技术栈的模块池生成效果不佳时，优先改进 `references/module-pool-generation.md` 的分层规则与范例，而不是加特例。

## License

MIT（见 [LICENSE](./LICENSE)）
