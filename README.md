# tech-stack-architect-coach

> 状态：`v0.1.0-alpha`。评估基准已定义，完整基线待跑；协议与文件结构可能继续调整。

**通用技术栈实战私教 Agent Skill** —— 你说"我想学 Redis / Kafka / MySQL / ES"，或"想学 Vue / React / Flutter"，它就启动一位大厂架构师学习教练：先诊断你的基础，再和你协商学习路线，确认后才开讲；模块化教学 + 面试闭环收尾，并通过 CONTEXT.md 交接文档支持跨窗口无损续接。

技术栈无关。Skill 不预置任何具体技术的教学内容，知识模块池由教练在学习意图发生时按协议**动态生成**。适用面覆盖程序员常见开发技术栈：后端、前端、移动端、桌面、大数据与 SRE 工具链——框架不变，项目载体与验证手段随"载体类型"配置自动适配。

## 与"直接甩一份学习大纲"的区别

| 常见做法 | 本 Skill |
|---|---|
| 假设学员知道自己基础和目标 | 用场景化提问**主动摸底**（不问"你什么水平"） |
| 大纲写死，直接开讲 | 生成路线草案 → **学员确认后才教学** |
| 按固定 Day 组织 | **动态模块池**：按依赖分层、带通过标准、按目标加权排序 |
| 单窗口对话，窗口一关状态全丢 | 上下文监控 + 冷启动五检查 + 双文档跨窗口续接：CONTEXT.md（状态快照，阶段性全文覆盖）+ TRACE.md（累积留痕/术语表，随时追加） |
| 教学约定写死 | 代码生成方式 / 节奏 / 深度天花板 / 笔记风格等全部**可配置** |

## 设计架构（渐进式披露）

```
SKILL.md（常驻：角色 + 硬约束 + 入口流程）
  │
  ├─ 收到学习意图 → references/diagnosis-protocol.md（三步诊断 + 配置采集）
  ├─ 诊断完成     → references/module-pool-generation.md（动态生成模块池 ★核心）
  ├─ 模块池就绪   → references/curriculum-negotiation-protocol.md（路线协商 + 确认门禁）
  ├─ 确认教学     → references/session-management.md（冷启动/监控/收尾）
  │               → references/teaching-preferences.md（偏好默认值）
  │               → references/context-template.md（交接文档：阶段性全文覆盖）
  │               → references/trace-template.md（留痕/术语表：随时追加）
  ├─ 教学过程中 → TRACE.md 追加（执行留痕 / 术语表）
  └─ 需要范例     → references/examples/（Redis 的 CONTEXT.md、Kafka 的路线草案）
```

## 安装

### Claude Code

```bash
# 项目级：放到你的项目里
cp -r tech-stack-architect-coach /path/to/your-project/.claude/skills/

# 或用户级：全局可用
cp -r tech-stack-architect-coach ~/.claude/skills/
```

### Codex / Cursor / 其他支持 SKILL.md 的平台

将 `tech-stack-architect-coach/` 目录整体放入对应平台的 skills 目录（如 Codex 的 `~/.codex/skills/`），或直接在新会话开始时粘贴 `SKILL.md` 全文 + 对应的 `references/` 文件内容。

> 本 Skill 的运行时依赖是"一个会读 Markdown 协议的 LLM Agent"，目录结构遵循 Anthropic Agent Skills 规范，任何实现了 frontmatter 触发 + 文件按需读取的平台均可使用。

## 使用示例

### 场景一：从零开始

> **你**：我想学 Kafka
>
> **教练**（加载诊断协议）：好，学 Kafka 是个不错的选择。在排学习计划之前，我先问你几个具体问题……（场景化摸底 → 目标确认 → 偏好采集 → 配置采集）
>
> **教练**（加载模块池生成协议）：根据你的情况，我生成了 12 个模块的 Kafka 模块池……（质量自检后进入协商）
>
> **教练**（加载协商协议）：路线草案如下，重点看总时长、模块取舍和最终产出，有要调整的吗？
>
> **你**：可以，按这个来。
>
> **教练**：确认收到。现在开始模块 0：环境搭建与验证。第一步……（进入六步走教学）

### 场景二：跨窗口续接（第 3 天）

> **你**：（新窗口）说"继续学 Redis"——**必须带上技术栈名称**，否则 Skill 可能触发不了（纯"继续"二字没有足够的学习意图信息）
>
> **教练**（第 0 步自动读取项目根目录的 CONTEXT.md 与 TRACE.md，随后冷启动五检查）：CONTEXT.md 和 TRACE.md 已读完。环境确认：Redis 容器 6379 在线，TRACE.md 留痕最后一次变更是 M4 的 `CONFIG SET save`（重启会失效，不影响当前）。术语表 7 条已核对，不会重复解释。偏好按之前确认的：mixed + 每步确认 + 面试高压。路线方向核对：仍按"面试冲刺优先、M4 提前"的 v2 决议走，无偏离。当前进度：模块 4/12（RDB 与 AOF），六步走第 ④ 步。遗留疑问：AOF rewrite 期间的写命令处理，计划在本模块面试环节解决。
>
> 现在按【待办锁定】继续：你执行 RdbInspector，把 bgsave 期间写操作是否阻塞的结果贴给我。
>
> （若所用平台读不到文件——如纯网页对话——教练会退一步请你粘贴 CONTEXT.md 全文，流程不变。）

### 场景三：一句话用法提问（不触发）

> **你**：redis 的 SETNX 命令怎么用？
>
> （不触发本 Skill。这是命令用法提问，不是系统学习意图——由普通问答处理。）

## 一条完整的模块教学实录（Redis 模块 1 · 节选）

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

## 目录结构

```
tech-stack-architect-coach/
├── SKILL.md
├── README.md
├── LICENSE
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
    └── eval-cases.md
```

## 协议文件速览

| 文件 | 一句话职责 |
|---|---|
| SKILL.md | 入口：识别学习意图，按阶段门禁调度各协议 |
| diagnosis-protocol.md | 场景化摸底（不问自评）+ 目标确认 + 偏好采集 + 占位符配置采集 |
| module-pool-generation.md | 为任意开发技术栈动态生成 8-14 个分层模块（★核心创新点；含 Redis/Kafka/MySQL/Vue 四份范例） |
| curriculum-negotiation-protocol.md | 呈现路线草案，学员确认前禁止教学；执行中变更需重新协商 |
| session-management.md | 冷启动五检查 + 显式锚点上下文预警 + 三交付收尾 |
| teaching-preferences.md | 把原模板写死的教学约定抽成 8 个可配置项 |
| context-template.md | 跨窗口状态快照载体：14 个固定区段（含【路线决议】防方向跑偏；留痕/术语经【留痕与术语指针】指向 TRACE.md，阶段性全文覆盖） |
| trace-template.md | 追加式 TRACE.md 模板：执行留痕 + 按模块分组的术语表（教学过程中随时追加，不全文重写） |
| examples/ | 格式范例（不是教学内容） |

## 跨平台兼容性

- **Claude Code**：`.claude/skills/` 目录加载，frontmatter 自动触发；CONTEXT.md 与 TRACE.md 都落盘在统一项目根目录（与代码同级），冷启动时教练直接用文件工具读取，无需学员粘贴。
- **Codex / Cursor CLI**（有文件工具）：同上，自动读取项目根目录的 CONTEXT.md 与 TRACE.md。
- **纯对话平台**（网页版等无文件访问能力）：教练读不到文件时会自动退一步，请学员粘贴 CONTEXT.md 全文（TRACE.md 只按需粘贴术语表与最近留痕），其余流程无损。
- **Codex / Cursor**（粘贴模式）：粘贴 SKILL.md 全文作为 Rules，references 按需粘贴。
- **平台无关降级**：任何支持 system prompt 的 Agent，按 SKILL.md 的加载地图手动提供对应协议文件即可，功能无损。

## 评估

见 `evals/eval-cases.md`：采用 `with_skill` vs `without_skill` 的 A/B 对照，用 Skill Lift 衡量本 Skill 在路线合理性、诊断质量、会话续接正确率上的增益。

## 贡献

- 改进协议文件：直接提 PR，注意保持"通用框架"定位——**不要往 Skill 里加具体技术栈的预置教学内容**（模块池是运行时生成的）。
- 新增技术栈的模块池生成效果不佳时，优先改进 `module-pool-generation.md` 的分层规则与范例，而不是加特例。

## License

MIT（见 LICENSE）

