# memcached在数据删除方面有效利用资源

<!-- create time: 2016-03-17 16:30:23  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->


### 数据不会真正从memcached中消失  

上次介绍过， `memcached`不会释放已分配的内存。记录超时后，客户端就无法再看见该记录（invisible，透明）， 其存储空间即可重复使用。
### Lazy Expiration

`memcached`内部不会监视记录是否过期，而是在`get`时查看记录的时间戳，检查记录是否过期。 这种技术被称为`lazy（惰性）expiration`。因此，`memcached`不会在过期监视上耗费CPU时间。
### LRU：从缓存中有效删除数据的原理

`memcached`会优先使用已超时的记录的空间，但即使如此，也会发生追加新记录时空间不足的情况， 此时就要使用名为 Least Recently Used（LRU）机制来分配空间。 顾名思义，这是删除“最近最少使用”的记录的机制。 因此，当`memcached`的内存空间不足时（无法从slab class 获取到新的空间时），就从最近未被使用的记录中搜索，并将其空间分配给新的记录。 从缓存的实用角度来看，该模型十分理想。

不过，有些情况下`LRU`机制反倒会造成麻烦。`memcached`启动时通过“`-M`”参数可以禁止`LRU`，如下所示：

    $ memcached -M -m 1024

启动时必须注意的是，小写的“`-m`”选项是用来指定最大内存大小的。不指定具体数值则使用默认值64MB。

指定“`-M`”参数启动后，内存用尽时`memcached`会返回错误。 话说回来，`memcached`毕竟不是存储器，而是缓存，所以推荐使用`LRU`。