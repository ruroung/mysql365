15.1 Introduction to InnoDB

InnoDB是一个通用用途的存储引擎，它平衡了高可靠和高性能。
MySQL 8.0，InnoDB是默认存储引擎。
通过create table语句默认创建的表是InnoDB表。

InnoDB的关键优势
    1.支持事务ACID。
        DML操作遵循ACID模型，具备事务提交、回滚、宕机恢复的能力，从而保护用户数据。
    2.多读并发，高性能。
        低级别的锁和Oracle风格的一致性读，从而增强多用户的并发能力和高性能。
    3.支持主键查询，查询性能好，最小化的磁盘I/O。
        InnoDB表将用户数据放在磁盘并基于主键优化查询。每张InnoDB表都有一个被称为族索引的主键索引，通过它来组织数据以最小化主键查询时磁盘I/O。
    4.多表关联的完整一致性，通过外键实现。
        为了保证数据的完整性，InnoDB支持外键约束。

InnoDB的增强和新特性
待补充

额外的信息和资源
待补充

15.1.1 InnoDB表的好处
1.宕机恢复。不论硬件或软件错误，重启后InnoDB会自动恢复。
  宕机前已经commit的数据会重写入数据库。
  宕机前未commit的数据允许用户重启开始和继续。
2.InnoDB使用buffer pool，将表数据、索引数据缓存在主存。
  经常使用的数据直接在内存中处理。
  buffer pool缓存了各种各样的数据，加速了处理速度。
  专用服务器（指服务器只用于数据库服务），80%的物理内存被用于buffer pool。
3.外键，可以将多个表的数据进行关联，保证数据的引用完整性。
4.如果磁盘或内存中的数据出现了损坏，checksum机制会在你使用前发出警告。
  innodb_checksum_algorithm变量定义checksum算法。
5.当你设计数据库时，为每个表使用了适当的主键列，那么where、order by、group by、join操作都会被自动优化，会非常快。
6.change buffering机制能自动优化inserts、updates、deletes操作。
  InnoDB不仅仅允许并发读写，还缓存了被修改过的数据。
7.当同一个表的相同行被反复访问时，自适应哈希Adaptive Hash Index会接管这些查询使得更快，就好像是在访问哈希表一样。
8.你可以压缩表和对应的索引。

加密

create drop 

truncate

BLOB、long text fields

INFORMATION_SCHEMA

PERFORMANCE_SCHEMA

mix tables

InnoDB表能够处理非常大量的数据，即使操作系统的文件大小限制为2GB。

15.1.2 InnoDB表的最佳实践

15.1.3 InnoDB作为默认存储引擎

15.1.4 InnoDB测试和基准测试



15.3 InnoDB Multi-Versioning

InnoDB是一个多版本存储引擎。它保持将要被改动的行的旧版本数据，从而支持事务特性，例如并发和回滚。
这些信息存储在数据结构rollback segment的undo tablespaces中。
InnoDB使用rollback segment中的信息实现undo操作。
同样地，使用这些信息来构建更早版本的信息来实现一致性的读。

在InnoDB内存，每行增加3个fields。

1.6字节的DB_TRX_ID   事务标识符
2.7字节的DB_ROLL_PTR   指向undo log record (rollback segment)
3.6字节的DB_ROW_ID

源码在
ha_innodb.cc
[[nodiscard]] inline int create_table_info_t::create_table_def( 


多版本设计，当通过SQL删除一个行并没有立刻从物理上删除这个行。
InnoDB物理删除时依据行和它的索引记录，当这些内容在undo log中删除。
这种删除方式称为purge，很快，它的操作和SQL语句的删除是一样顺序。

Multi-Versioning and Secondary Indexes
MVCC处理secondary index和clustered index不同。


15.4 InnoDB In-Memory Structures

15.5 InnoDB In-Memory Structures

15.5.1 Buffer Pool
buffer pool被分割成页，每页可以放入多个行。
buffer pool是通过page链表，通过LRU算法管理。

LRU算法
new sublist
old sublist
midpoint insertion

old sublist被候选为待驱逐的页。
算法步骤：
1.3/8的页是old sublist页。
2.midpoint作为newlist和oldlist的分界点。
3.访问数据，会使数据更年轻，放到new sublist头部。


insert buffer
ibuf
change buffer
是同一个东西。

change buffer 存磁盘在system tablespace