# Redis 性能分析

## 1. Redis 慢日志

low log 是 Redis 用来记录查询执行时间的日志系统。

查询执行时间指的是不包括像客户端响应(talking)、发送回复等 IO 操作，而单单是执行一个查询命令所耗费的时间。

```bash
redis> SLOWLOG GET   # 显示慢日志
redis> SLOWLOG LEN   # 慢日志记录数量
redis> SLOWLOG RESET  # 清空慢日志
```



**慢日志的配置**

选项一 : `slowlog-log-slower-than` ，它决定要对执行时间大于多少微秒(microsecond，1秒 = 1,000,000 微秒)的查询进行记录。

选项二：`slowlog-max-len` ，它决定 slow log *最多*能保存多少条日志， slow log 本身是一个 FIFO 队列，当队列大小超过 `slowlog-max-len` 时，最旧的一条日志将被删除，而最新的一条日志加入到 slow log ，以此类推。

```bash
redis> CONFIG GET slowlog-log-slower-than
redis> CONFIG SET slowlog-log-slower-than 100
```

## 2. Redis info

```bash
INFO [section]
```

以一种易于解释（parse）且易于阅读的格式，返回关于 Redis 服务器的各种信息和统计数值。

通过给定可选的参数 `section` ，可以让命令只返回某一部分的信息：

- `server` : 一般 Redis 服务器信息，包含以下域：

  > - `redis_version` : Redis 服务器版本
  > - `redis_git_sha1` : Git SHA1
  > - `redis_git_dirty` : Git dirty flag
  > - `os` : Redis 服务器的宿主操作系统
  > - `arch_bits` : 架构（32 或 64 位）
  > - `multiplexing_api` : Redis 所使用的事件处理机制
  > - `gcc_version` : 编译 Redis 时所使用的 GCC 版本
  > - `process_id` : 服务器进程的 PID
  > - `run_id` : Redis 服务器的随机标识符（用于 Sentinel 和集群）
  > - `tcp_port` : TCP/IP 监听端口
  > - `uptime_in_seconds` : 自 Redis 服务器启动以来，经过的秒数
  > - `uptime_in_days` : 自 Redis 服务器启动以来，经过的天数
  > - `lru_clock` : 以分钟为单位进行自增的时钟，用于 LRU 管理

- `clients` : 已连接客户端信息，包含以下域：

  > - `connected_clients` : 已连接客户端的数量（不包括通过从属服务器连接的客户端）
  > - `client_longest_output_list` : 当前连接的客户端当中，最长的输出列表
  > - `client_longest_input_buf` : 当前连接的客户端当中，最大输入缓存
  > - `blocked_clients` : 正在等待阻塞命令（BLPOP、BRPOP、BRPOPLPUSH）的客户端的数量

- `memory` : 内存信息，包含以下域：

  > - `used_memory` : **由 Redis 分配器分配的内存总量，以字节（byte）为单位**
  > - `used_memory_human` : **以人类可读的格式返回 Redis 分配的内存总量**
  > - `used_memory_rss` : **从操作系统的角度，返回 Redis 已分配的内存总量（俗称常驻集大小）。这个值和 `top` 、 `ps` 等命令的输出一致。** 
  > - `used_memory_peak` : Redis 的内存消耗峰值（以字节为单位）
  > - `used_memory_peak_human` : 以人类可读的格式返回 Redis 的内存消耗峰值
  > - `used_memory_lua` : Lua 引擎所使用的内存大小（以字节为单位）
  > - `mem_fragmentation_ratio` : **`used_memory_rss` 和 `used_memory` 之间的比率**
  > - `mem_allocator` : 在编译时指定的， Redis 所使用的内存分配器。可以是 libc 、 jemalloc 或者 tcmalloc 。
  >
  > 在理想情况下， `used_memory_rss` 的值应该只比 `used_memory` 稍微高一点儿。
  >
  > `peak` 内存使用峰值，`rss`相当于规划使用内存，`used`相当于实际使用量。
  >
  > 当 `rss > used` ，且两者的值相差较大时，表示存在（内部或外部的）内存碎片。
  >
  > 内存碎片的比率可以通过 `mem_fragmentation_ratio` 的值看出。
  >
  > 当 `used > rss` 时，表示 Redis 的部分内存被操作系统换出到交换空间了，在这种情况下，操作可能会产生明显的延迟。
  >
  > Because Redis does not have control over how its allocations are mapped to memory pages, high `used_memory_rss` is often the result of a spike in memory usage.
  >
  > 当 Redis 释放内存时，分配器可能会，也可能不会，将内存返还给操作系统。
  >
  > 如果 Redis 释放了内存，却没有将内存返还给操作系统，那么 `used_memory` 的值可能和操作系统显示的 Redis 内存占用并不一致。
  >
  > 查看 `used_memory_peak` 的值可以验证这种情况是否发生。

- `persistence` : `RDB` 和 `AOF` 的相关信息

- `stats` : 一般统计信息

  > - `total_commands_processed` : 显示了Redis服务处理命令的总数，其命令都是从一个或多个Redis客户端请求过来的。

- `replication` : 主/从复制信息

- `cpu` : CPU 计算量统计信息

- `commandstats` : Redis 命令统计信息

- `cluster` : Redis 集群信息

- `keyspace` : 数据库相关的统计信息

除上面给出的这些值以外，参数还可以是下面这两个：

- `all` : 返回所有信息
- `default` : 返回默认选择的信息

当不带参数直接调用 [INFO](http://doc.redisfans.com/server/info.html#info) 命令时，使用 `default` 作为默认参数。