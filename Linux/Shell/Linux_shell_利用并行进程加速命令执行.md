### Linux_shell_利用并行进程加速命令执行

对于多核的 CPU 只有使用的软件可以利用其多核特点才不会将大量的计算运行在其中一个核心上, 否则其他的核心都会被闲置。这里我们看一下如何让命令运行得更快。

#### 实战演练

以 `md5sum` 为例, 由于涉及运算, 该命令属于 CPU 密集型命令。如果多个文件需要生成校验和, 我们可以使用下面的脚本来运行 `md5sum` 的多个实例。

    #!/bin/bash
    # 文件名 : generate_checksums.sh
    PIDARRAY=()
    for file in File1.iso File2.iso
    do
        md5sum $file &
        PIDARRAY+=("$!")
    done
    wait ${PIDARRAY[@]}
    
运行后, 输出结果和下面命令是一样的 : 

    md5sum File1.iso File2.iso
    
但因为多个 `md5sum` 命令是同时运行的, 如果你使用的是多核处理器, 就会更快地获得运行效果(可以使用 `time` 命令来验证)。

#### 工作原理

我们利用了 Bash 的操作符 &, 它使得 shell 将命令置于后台并继续执行脚本。这意味着一旦循环结束, 脚本就将退出, 而 `md5sum` 命令仍在后台运行。为了避免这种情况, 我们使用 `$!` 来获得进程的 PID, 在 Bash 中, `$!` 保存着最近一个后台进程的 PID。我们将这些 PID 放入数组, 然后使用 `wait` 命令等待这些进程结束。