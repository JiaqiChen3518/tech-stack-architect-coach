# 范例：已填写的 CONTEXT.md（Redis 场景，第 3 个窗口续接时）

> 本文件是**格式范例**，展示一份真实使用中的交接文档长什么样。
> 用途：① 冷启动时与学员持有的实际 CONTEXT.md 对齐格式；② 演示各区段的填写颗粒度。

---

````markdown
# CONTEXT.md — Redis 实战私教交接文档

<!-- 本文件是学习状态的唯一权威来源。默认与 redis-architect-lab 项目同根目录存放；新窗口开启时教练会主动读取本文件（读不到时才需学员全文粘贴）。 -->

## 【技术栈配置】
- 技术栈名称：Redis
- 技术栈简称：redis
- 统一项目名：redis-architect-lab
- 载体类型：后端服务
- 核心目标（业务痛点）：缓存穿透 / 库存超卖 / 大 Key 阻塞
- 主语言/框架：Java 21 / Spring Boot 3.2.5
- 复现语言：Java —— 源码红线"伪代码复现"默认用主语言
- 客户端库：Lettuce 6.3.2 / Redisson 3.27.2
- 类比锚点：MySQL（B+树、Buffer Pool、MVCC、binlog）
- 源码红线语言：C 语言 —— 禁止引导阅读/编译，底层只允许 Java 伪代码 + ASCII 图

## 【学员画像】
- 基础水平：用过（能熟练用五种数据结构做缓存，但说不清 RDB/AOF 取舍和线程模型）
- 项目背景：电商中台 Java 开发，QPS 峰值 3 万，正在推动缓存层改造
- 学习目标：面试冲刺（两周后有美团二面）+ 项目落地（缓存改造方案要过评审）
- 每周可投入：5 天

## 【教学偏好】
- 代码生成方式: mixed
- 代码落盘方式: write_to_disk
- 教学节奏: step_confirm
- 深度天花板: business_arch
- 深度天花板确认阶段: 定制时已确认 2026-09-05
- 面试追问强度: high_pressure
- 笔记风格: interview_oriented
- 环境验证频率: per_module
- 类比锚点: MySQL（B+树、MVCC、binlog）
- 确认日期: 2026-09-05

## 【路线决议】
- 学习目标：面试冲刺（两周后美团二面，权重最高）+ 项目落地（缓存改造方案过评审）
- 排序逻辑：面试权重高的持久化/锁模块往前提；项目评审急需的大 Key 治理紧跟缓存三大坑
- 已确认调整：分布锁拆出 Redisson 单独成模块（学员要求深挖，应对二面手撕）；布隆过滤器与缓存三大坑合并为一个 1 天模块（压缩总时长）
- 关闭的可选项：~~跳过基础层直接进 L3~~（学员为保基础牢固选择保留）；~~增加 Cluster 集群模块~~（时间不够，搁置为可选补充）
- 草案版本与确认日期：v2 · 2026-09-05

## 【环境快照】
| 项目 | 值 |
|---|---|
| 宿主 OS | Windows 11（WSL2 Ubuntu 22.04） |
| Docker Engine | 27.0.3 |
| Redis | 7.2.5（容器名 redis-lab） |
| Java | JDK 21 / Maven 3.9.6 / Spring Boot 3.2.5 / Lettuce 6.3.2 |
| 端口 | Tomcat 8080 / Redis 6379 |
| 验证命令 | `docker ps` 见 redis-lab；`redis-cli -p 6379 PING` 返回 PONG |

> 说明：本快照每格值均来自学员 M0 阶段的实测输出（如宿主 OS 由 `cat /etc/os-release` 贴回、Docker 版本由 `docker --version` 确认），教练未凭经验预填。

## 【模块进度】
| 顺序 | 模块 | 层 | 预估时长 | 状态 | 完成日期 |
|---|---|---|---|---|---|
| 1 | 五大数据结构实战 | L1 | 1 天 | ✅ 已完成 | 2026-09-06 |
| 2 | Pipeline 与序列化 | L1 | 0.5 天 | ✅ 已完成 | 2026-09-06 |
| 3 | 线程模型与 IO 多路复用 | L1 | 0.5 天 | ✅ 已完成 | 2026-09-07 |
| 4 | RDB 与 AOF 持久化 | L2 | 1 天 | 🔵 进行中（六步走第 ④ 步） | - |
| 5 | 缓存三大坑 | L3 | 0.5 天 | ⬜ 未开始 | - |
| 6 | 布隆过滤器 | L3 | 0.5 天 | ⬜ 未开始 | - |
| 7 | 大 Key / 热 Key 治理 | L3 | 0.5 天 | ⬜ 未开始 | - |
| 8 | 分布式锁与 Redisson | L4 | 1 天 | ⬜ 未开始 | - |

## 【当前进度】
- 当前模块：M4 RDB 与 AOF 持久化
- 六步走位置：第 ④ 步编码落地（学员刚拿到 RdbInspector 类，尚未执行）
- 已完成知识点列表：五大数据结构与场景、Pipeline 批处理、RESP 协议与 RTT、IO 多路复用与 Reactor、单线程模型的优劣
- 已跑通的验证：Pipeline 10 万次写入 1.2s vs 非 Pipeline 38s；redis-benchmark GET QPS 9.8 万

## 【项目文件树】
```
redis-architect-lab/
├── src/main/java/com/example/redislab/
│   ├── datastructure/   # M1：StringHashListSetZsetDemo ×5
│   ├── pipeline/        # M2：PipelineBenchmark
│   └── persistence/     # M4：RdbInspector（进行中）
└── docs/
    ├── redis M1 五大数据结构.md
    ├── redis M2 Pipeline与序列化.md
    └── redis M3 线程模型.md
```

## 【已有知识】
- MySQL binlog 与 redo log 的两阶段提交：~/notes/mysql-logs.md（讲解持久化时可直接类比，无需重讲）

## 【留痕与术语指针】
完整执行留痕：见 ./TRACE.md 的【执行留痕】（Append 追加）

完整术语表：见 ./TRACE.md 的【已讲术语表】（按模块分组）

本次冷启动需重点核对：[M4] `CONFIG SET save "60 1000"` 重启会失效，当前实例未受影响

## 【待办锁定】
~~M4 第④步：教练交付 RdbInspector 类~~ -> **M4 第④步：学员执行 RdbInspector，贴出 bgsave 期间的写操作是否阻塞的结果**

## 【遗留疑问】
- AOF rewrite 期间的写命令如何处理？（计划 M4 面试追问环节解决，若仍不清楚记入下次收尾）

## 【严格约束】
- 统一项目只维护一个：redis-architect-lab，旧代码一律保留不删
- Redis 端口固定 6379，本机无其他 Redis 实例
- 源码红线：C 语言源码不可引导阅读，底层只允许 Java 伪代码 + ASCII 图
- 深度天花板：business_arch（到业务架构层为止，不碰源码实现细节）；教学过程中不可单方面切换
- 教学约定（2026-09-05 学员确认）：教学视角=业务架构+面试考点；抛出问题后直接给答案；教练交付代码必须先 mvn compile 自验；基础概念首次出现需一句话解释并用实测数据佐证

## 【会话状态】
- 窗口序号：3
- 最近收尾时间：2026-09-07
- 上次收尾时的模块进度：M4 RDB 与 AOF 持久化，六步走第 ④ 步（编码落地中）
- 本窗口起始主题：M4 第④步——学员执行 RdbInspector，验证 bgsave 期间写操作是否阻塞
- 本窗口已进行回合数：12（软触发参考窗口 25-30；30-40 为硬上限参考）

<!-- CONTEXT_END -->
````
