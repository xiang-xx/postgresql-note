# part3 - 服务端管理

安装方法：
- 源码安装
- 二进制
- apt 等其他工具等

启动设置与运维
- 如何创建一个数据库集群，这里的集群 cluster 值得是单机上的集群，多个 database.
- 启动命令，指定数据位置，日志等
- 停止，更新等操作
- 使用 SSL 加密传输


服务端配置
- 资源使用配置
  - heap, stack
  - transction count
  - 共享内存
  - 磁盘
  - 后台写入，异步刷盘等
- WAL -- write ahead log 配置
  - 刷盘
  - 压缩
  - 缓冲区
  - checkpoint
  - 恢复
- 备份：内置的 streaming replication 功能
  - 主库备库的 wal，恢复，接收超时等配置
- 查询计划
  - 并行查询，join, pool size 等
- 错误上报和日志：配置哪些场景下上报错误，错误级别，采样，以及错误写到哪里，日志的配置
- 运行时静态分析：在运行时统计数据库表数据，比如运行时跟踪总行数
- 客户端连接配置，超时，本地化，锁等
- 锁管理，锁超时，最大锁数量
- 错误处理
- 预设选项，块大小，最大函数参数，最大id长度，最大 index keys，server 编码，版本

客户端授权
- pg_hba.conf 配置文件控制客户端授权，哪些主机地址能访问哪些数据库，需要哪些授权等
- 授权方法
  - 信任
  - 密码
  - GASAPI
  - LDAP
  - 证书等

数据库角色：类似用户组

本地化
- 字符集，排序
- 语言
- message
- 货币
- 数字
- 日期和时间

备份和恢复
- sql dump
- 文件系统级别备份
- 指定时间点恢复。利用 WAL


高可用，负载均衡和备份
- 共享磁盘故障转移，多个独立共享磁盘，一个 server crash 了，另一个使用相同的磁盘继续服务
- File System（块设备）备份：镜像文件系统
- 基于 WAL 备份
- 逻辑备份：主库基于 wal 生成 逻辑数据修改流
- Trigger-Based Primary-Standby Replication 把数据修改的语句在备库执行
- SQL-Based Replication Middleware  基于 sql 备份。random CURRENT_TIMESTAMP 之类的语句要被修改后才能使用
- 异步多主机复制
- 同步多主机复制
- 数据分区：表分为多个 data sets，每个 set 只能被一个 server 修改。server 可以位于任何地方。
- 多 server 并行查询执行

监控数据库
- unix 工具，ps psql 等
- 静态统计，pg_stat 等相关系统表
- 动态 tracing

磁盘使用监控
```sql
SELECT pg_relation_filepath(oid), relpages FROM pg_class WHERE  relname = 'customer';
pg_relation_filepath | relpages
----------------------+----------
 base/16384/16806     |       60
```

可靠性和 Write-Ahead Log
- disk cache：能够加快落盘速度，但可能因为故障导致落盘失败。
- WAL，修改数据（表和索引的数据文件）之前必须要落盘的日志（对表和索引数据文件修改的日志）。 -- 随机 io 改为顺序 io
- 异步提交。降低可靠性，加快事务提交速度。

逻辑备份：
- publication
- subscription：逻辑备份可能出现冲突，导致备份停止

JIT Just-in-Time Compilation
- server 会把一些语句转换为本地函数执行

回归测试，支持使用命令对 server 回归测试。
