### Memcache_命令选项及配置

**语法**  
       memcached [options]

**描述**  
       memcached 是一个灵活的内存对象缓存守护进程。它通过将对象缓存在内存中，从而降低WEB应用对数据库的压力。
       它基于 libevent 库，可以伸缩到任意大小，并永远使用非阻塞的网络I/O。
       因此在使用 memcached 的机器上应避免使用虚拟内存(swap)。

选项
       这个守护进程不使用任何配置文件，只使用下面的命令行选项控制其行为。

       -s file
              指定在那个文件上监听 Unix socket (不使用TCP/IP网络)

       -a perms
              指定"-s"选项创建的 Unix socket 文件的权限(八进制)

       -l ip_addr
              在指定的 ip_addr 上监听，默认是所有可用地址(INADDR_ANY)。
              这是一个重要的选项，因为没有其它更多的访问控制方法。
              出于安全考虑，建议绑定到内网接口或者有防火墙保护的网络接口。

       -d     以守护进程的方式运行(后台运行)

       -u username
              指定以 username 用户的身份运行，该选项仅在以root用户启动时有效。

       -m num
              使用 num MB 大小的内存作为缓冲区，默认值是 64MB

       -c num
              最大允许 num 个并发连接，默认值是 1024

       -R num
              这是一个为了防止某些客户端被饿死而设置的选项。num 的默认值是"20"。
              参数 num 表示服务器在同一个连接内最多连续处理 num 个请求。
              一旦某连接连续处理的请求数超过了 num 的限制，服务器将会转而去处理其他连接的请求，
              直到其他连接的请求全部处理完毕(或者也达到了上限)之后，才会回过头来继续处理此连接上剩余的请求。

       -k     锁定所有分页内存(paged memory)。这个选项在缓冲区比较大的时候使用可能会有些危险。

       -p num
              在TCP端口 num 上监听，默认值是 11211

       -U num
              在UDP端口 num 上监听，默认值是 11211 ，0 表示关闭

       -M     禁止在缓冲区不够用的时候自动移除缓存对象。这样将会导致缓冲区满时无法添加新对象。

       -r     将核心文件尺寸限制加大到最大值

       -f factor
              将 factor 用作计算每个缓存项所占内存块大小的乘数(multiplier)。默认值是"1.25"。
              也许较小的乘数会减少内存的浪费，但实际效果取决于可用内存总量以及不同缓存项大小的分布状况。

       -n size
              最少为每个缓存对象的"key, value, flags"分配 size 字节。默认值为 48 。
              如果你有大量的小"key,value"对，减小此值将会大大提高内存的利用效率。
              另一方面，如果你使用"-f"选项指定了较大的块膨胀系数(chunk growth factor)，
              那么你应当增加此值以让更高比例的缓存项更适合稠密的压缩内存块。

       -C     禁止使用CAS(可以为每个缓存项节约8个字节的空间)

       -h     显示版本号和选项列表后退出

       -v     在事件循环过程中打印详细的错误和警告消息

       -vv    显示更加详细的消息，也就是在"-v"的基础上打印客户端命令和应答

       -i     打印 memcached 和 libevent 的许可证

       -P filename
              将进程号(PID)保存在 filename 中，仅在使用了"-d"选项后才有意义。

       -t threads
              使用 threads 个线程来处理接入请求。将此值设置为超过总的CPU核心数只会弄巧成拙。默认值是 4 。

       -D char
              使用 char 作为key前缀和ID之间的分隔符，这将用于每一个前缀状态报告。默认值是冒号(:)。
              明确指定此选项后状态收集器将被自动打开，否则可以通过向服务器发送"stats detail on"命令来开启。

       -L     尽量使用大内存页(如果可用)。使用大内存页可以增大TLB缓存命中概率，从而提升内存性能。
              此选项仅在内核支持大内存页(CONFIG_TRANSPARENT_HUGEPAGE,CONFIG_HUGETLBFS)的系统上有意义。

       -B proto
              指定要使用的绑定协议。默认值"auto"表示由服务器与客户端进行协商。
              而"ascii"和"binary"则明确表示仅允许使用确定的协议。

       -I size
              指定每个slab/slub页的默认大小。默认值是"1m"。允许的最小值是"1k"，允许的最大值是"128m"。
              改变此项同时也改变了每个缓存项的尺寸上限。
              增大此项的同时也增大了slab/slub页的数量(可以使用 -v 查看)，以及 memcached 的内存总使用量。

       -F     禁用"flush_all"命令。
              cmd_flush 计数器仍然会增加，但是客户端将会收到"刷新动作未被执行"的错误消息。

       -o options
              逗号分隔的扩展或实验性选项的列表。参见 -h 或 wiki 以获取这些选项。



 1、stats： memcached 实例的当前统计数据。
    
    STAT pid 22459                             进程ID
    STAT uptime 1027046                        服务器运行秒数 
    STAT time 1273043062                       服务器当前unix时间戳 
    STAT version 1.4.4                         服务器版本 
    STAT pointer_size 64                       操作系统字大小(这台服务器是64位的) 
    STAT rusage_user 0.040000                  进程累计用户时间 
    STAT rusage_system 0.260000                进程累计系统时间 
    STAT curr_connections 10                   当前打开连接数 
    STAT total_connections 82                  曾打开的连接总数 
    STAT connection_structures 13              服务器分配的连接结构数 
    STAT cmd_get 54                            执行get命令总数 
    STAT cmd_set 34                            执行set命令总数 
    STAT cmd_flush 3                           指向flush_all命令总数 
    STAT get_hits 9                            get命中次数 
    STAT get_misses 45                         get未命中次数 
    STAT delete_misses 5                       delete未命中次数 
    STAT delete_hits 1                         delete命中次数 
    STAT incr_misses 0                         incr未命中次数 
    STAT incr_hits 0                           incr命中次数 
    STAT decr_misses 0                         decr未命中次数 
    STAT decr_hits 0                           decr命中次数 
    STAT cas_misses 0                          cas未命中次数 
    STAT cas_hits 0                            cas命中次数 
    STAT cas_badval 0                          使用擦拭次数 
    STAT auth_cmds 0 
    STAT auth_errors 0 
    STAT bytes_read 15785                      读取字节总数 
    STAT bytes_written 15222                   写入字节总数 
    STAT limit_maxbytes 1048576                分配的内存数（字节） 
    STAT accepting_conns 1                     目前接受的链接数 
    STAT listen_disabled_num 0                 
    STAT threads 4                             线程数 
    STAT conn_yields 0 
    STAT bytes 0                               存储item字节数 
    STAT curr_items 0                          item个数 
    STAT total_items 34                        item总数 
    STAT evictions 0                           为获取空间删除item的总数
