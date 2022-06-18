
https://dev.mysql.com/doc/refman/8.0/en/performance-schema.html


pfs功能特点

1. 使用performance schema存储引擎，使用performance schema数据库存储数据。
深入运行时的内部执行情况，关注运行时的性能相关数据。
而information schema关注的是元数据（metadata）。

2. performance schema数据来源于事件
事件可以是：
函数的执行时间
操作系统的等待
SQL执行的整个过程的各个步骤的情况
同步、互斥量
table I/O
table lock
存储引擎相关

3. 不同于binlog、Event schedule events

4. performance schema虽然是一个数据库实例和数据表，但它们的调整不会记录binlog和被复制备份。

5. Current events are available, as well as event histories and summaries. 
你可以决定事件事件设施的工作次数和时长。

6. 在源代码中，performance schema 引擎通过事件设施(instrumentation points)收集数据。

7. 数据记录在performance schema表中，可以通过select获取这些数据。

8. 通过SQL可以动态修改performance schema的配置，立刻生效。

9. performance schema数据记录在内存，不会记录到磁盘。
产生的数据来自服务启动以来，服务停止就消失了。

10. performance schema所有平台都支持。

11. 通过修改设施的源代码调整数据收集。

设计目标：
1. performance schema的运行对服务是无影响的，无感的，透明的。

2. 持续、谨慎的监控对服务的影响非常有限，不会导致服务不可用。

3. 不用调整Parser。不新增关键字、语句等。

4. performance schema的执行失败会控制在内部，不影响服务的正常运行。

5. 对于立刻收集数据和延迟取回数据，优先级应该是先快速收集。
（言下之意应该是会收集冗余数据）

6. performance schema表页有索引。

7. It is easy to add new instrumentation points.
增加设施是简单的。

8. 设施具有版本区别管理。

