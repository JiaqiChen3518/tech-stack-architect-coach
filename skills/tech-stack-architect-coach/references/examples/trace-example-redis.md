# 范例：已填写的 TRACE.md（Redis 场景，第 3 个窗口续接时）

> 本文件是**格式范例**，展示一份真实使用中的 TRACE.md 长什么样。
> 用途：① 演示追加式留痕与术语表的填写颗粒度；② 与 `context-example-redis.md`（同场景的 CONTEXT.md）配套——CONTEXT.md 里只留【留痕与术语指针】指向本文件。
> **配套关系**：本范例对应 `context-example-redis.md` 的同一时刻（M4 进行中、第 3 窗口），两者内容互不重叠：状态在 CONTEXT.md，过程细节在本文件。

---

````markdown
# TRACE.md — Redis 执行留痕与术语表

<!-- 本文件只追加不重写。与 CONTEXT.md 同级存放。冷启动时教练按需读取最近 2 个模块留痕 + 全部术语。 -->

## 【执行留痕】

- [M0] 环境确认：`cat /etc/os-release` + `docker --version` → 学员自查贴回，确认宿主为 WSL2 Ubuntu 22.04、Docker 27.0.3（教练不预填/不猜，按 SKILL.md 1.5 红线 A 逐项确认后回填【环境快照】）
- [M0] `docker run -d --name redis-lab -p 6379:6379 -v /opt/redis/data:/data redis:7.2.5` → 启动课程 Redis 实例，数据目录挂载到宿主 /opt/redis/data，重启不丢数据
- [M0] `docker exec -it redis-lab redis-cli` 后执行 `CONFIG SET appendonly yes` → 开启 AOF，仅当前实例生效，已同步写入 redis.conf（见下条）
- [M0] 修改容器内 /usr/local/etc/redis/redis.conf：`appendonly yes`、`appendfsync everysec` → 使 AOF 配置持久化，重启仍生效
- [M1] `redis-benchmark -t get,set -n 100000 -q` → 压测单机 GET/SET 基线，实测 GET QPS 9.8 万，用于 M2 Pipeline 对比
- [M2] `redis-cli --pipe < commands.txt` 批量灌 10 万条 vs 循环单条 SET → Pipeline 1.2s vs 非 Pipeline 38s，验证批处理对 RTT 的摊薄
- [M4] `docker exec redis-lab redis-cli CONFIG SET save "60 1000"` → 临时调整 RDB 快照策略用于实验，重启后恢复默认（冷启动需提示学员该配置失效）

## 【已讲术语表】

### M0-M3
- RTT：命令往返时间，本机约 0.5ms
- RESP：Redis 序列化协议，文本协议，易解析但体积大于二进制协议
- Reactor：事件分发模式，Redis 用单线程 Reactor 处理 IO
- C10K：单机同时处理一万连接的问题，IO 多路复用是它的答案
- Pipeline：客户端打包多条命令一次发送，服务端按序执行后批量返回，摊薄多次 RTT
- 序列化：把对象转成可存储/传输的字节流，Redis 客户端常用 JSON / Protobuf / JDK 序列化
- IO 多路复用：单线程同时监听多个 fd，哪个就绪处理哪个（epoll/kqueue/select）
- bgsave：fork 子进程写 RDB 快照，主进程继续响应写命令

### M4
- RDB：内存数据的某一时刻全量快照文件，紧凑、恢复快，但故障点与上次快照之间数据丢失
- AOF：追加记录每条写命令的日志，丢失粒度更细（everysec 最多丢 1 秒），但体积大、恢复慢
- AOF rewrite：重写 AOF 文件压缩体积（合并过期 key、合并重复写），fork 子进程进行
- fsync：强制把 OS 缓冲刷到磁盘，`appendfsync everysec` 表示每秒一次
- fork：Linux 创建子进程的系统调用，子进程共享父进程内存页（写时复制 COW），Redis 持久化/重写都靠它
- COW（Copy-On-Write）：fork 后父子进程共享物理内存页，任一方写时才复制对应页，Redis 借此在持久化时不阻塞写
````

---

## 范例说明

- 本范例共 **7 条留痕**（M0 四条：环境确认一条 + 搭建/配置三条 + M1/M2 各一条 + M4 一条）、**14 条术语**（M0-M3 八条 + M4 六条），符合"4-6 条留痕、10-14 条术语"的颗粒度参考（本例因显式演示 M0 环境确认步骤略高于基准下限）；
- **冷启动读取范围示例**（本范例对应第 3 窗口续接）：教练读【已讲术语表】全部 14 条 +【执行留痕】最近 2 个模块（M2、M4）共 2 条；M0 的 4 条留痕不默认读取，但其中"redis.conf 持久化"一条若涉及环境判断可按需翻看；
- **留痕与 CONTEXT.md 的指针配合**：`context-example-redis.md` 的【留痕与术语指针】区段会写"本次冷启动需重点核对：[M4] CONFIG SET save 重启会失效"——这条指针就是从本范例【执行留痕】最后一条挑出来的关键变更；
- **追加而非重写**：第 3 窗口收尾时，教练只在【执行留痕】末尾补本模块新产生的命令、在【已讲术语表】补 M4 新讲术语，不动 M0-M3 的既有内容；CONTEXT.md 则全文覆盖更新。
