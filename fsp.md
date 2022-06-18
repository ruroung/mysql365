

表空间tablespace

系统表空间（System Tablespace）
独立表空间(File-per-table Tablespace)
通用表空间（General Tablespace）
undo表空间（Undo Tablespace）
临时表空间（Temporary Tablespace）


相关源码文件
fsp0file.cc
fsp0fsp.cc
fsp0space.cc
fsp0sysspace.cc


可以对应文件系统中的一个或多个文件




fsp0sysspace.cc

系统表空间、临时表空间的变量定义

/** The control info of the system tablespace. */
SysTablespace srv_sys_space;

/** The control info of a temporary table shared tablespace. */
SysTablespace srv_tmp_space;

通用表空间在fsp0space.cc

临时表空间在srv0start.cc创建
srv_open_tmp_tablespace

undo表空间的创建，有多个地方
innodb_create_undo_tablespace ha_innodb.cc
srv_undo_tablespaces_construct、srv_undo_tablespace_create srv0start.cc
fil_ibd_create trx0undo.cc

关键函数
SysTablespace::open_or_create

m_files数据怎么来的？

innodb_init
SysTablespace::parse_params

可以看到从innodb初始化得到的



/******************* fsp ****************/

fseg_create_general


/******************* fseg ****************/




/******************* xdes ****************/



/******************* page ****************/