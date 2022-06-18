相关源码文件

fil0types.h
fil0fil.h
fil0fil.cc

innodb对文件的管理，和数据库相关文件打交道


class Fil_system
    using Fil_shards = std::vector<Fil_shard *>;
    Fil_shards m_shards;

class Fil_shard
    using Spaces = std::unordered_map<space_id_t, fil_space_t *>;

    Spaces m_spaces;


struct fil_space_t

struct fil_node_t


创建space和相关文件

fil_space_create

fil_node_create
    Fil_shard::create_node



ib_file_suffix 这个枚举类型记录了各种文件后缀的枚举值
dot_ext 具体的指在这里记录

一些常量的定义

页面：
FIL_PAGE_*
FIL_PAGE_INDEX、FIL_PAGE_RTREE、FIL_PAGE_SDI等
页面类型的相关定义


页面中各数据字段的偏移
FIL_PAGE_SPACE_OR_CHKSUM
FIL_PAGE_OFFSET
FIL_PAGE_PREV
FIL_PAGE_SRV_VERSION
FIL_PAGE_TYPE
可以看到有些偏移量是一样的，应该是不同的FIL_PAGE_TYPE下，字段的复用。


通过页指针、字典偏移量、字段的数据字节数读取数据
例如通过以下接口：
mach_read_from_4
mach_write_to_4


通过FIL_PAGE_OFFSET字段等到的4字节数据是页编号，每个表空间最多有2^32个页，每个页有16KB，所以一个表空间最多能支持64T数据。

