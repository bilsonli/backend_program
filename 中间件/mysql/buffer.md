# buffer pool
用来缓存读写数据的，以页为单位(1页 16kb), 默认128M  
redo log保障了持久性  
修改或者读取不存在的记录时，会从磁盘读取到内存  

# change buffer
buffer pool中的一块，通常占25%，用来存储非唯一二级索引的修改，下次读取该记录时，再合并到buffer pool中去
