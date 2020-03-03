### shell 调试脚本

调试 shell 脚本不需要什么特殊工具。Bash 本身就包含了一些选项, 能够打印出脚本接受的参数和输入。

使用选项 `-x`, 启动跟踪调试 shell 脚本 : 

    $ bash -x script.sh
    
运行带有 `-x` 标志的脚本能打印出所执行的每一行命令以及当前状态。

`-x` 标识将脚本中执行过的每一行都输出到 stdout 。不过, 我们也可以要求只关注脚本某些部分的命令及参数的打印输出。针对这种情况, 可以在脚本中使用 `set built-in` 来启用或禁止调试打印。

- set -x: 在执行时显示参数和命令
- set +x: 禁止调试
- set -v: 当命令进行读取时显示输入
- set +v: 禁止打印输入。

        #!/bin/bash
        # debug.sh
        for i in {1..6}    #也可以写 {1,2,3,4,5,6}
        do
            set -x
            echo $i
            set +x
        done
        echo "Script executed"
        
上面脚本中, 仅在 -x 和 +x 所限制的区域内, `echo $i` 的调试信息才会被打印出来。

#### 重建调试信息输出格式

这种调试方法是 Bash 的内建功能提供的。它通常以固定的格式生成调试信息。但是在很多情况下, 我们需要以自定义格式显示调试信息。它可以通过传递 `_DEBUG` 环境变量来建立这类调试风格。

    #!/bin/bash
    function DEBUG()
    {
        [ "$_DEBUG" == "on" ] && $@ || :    #如果_DEBUG等于on 并且 DEBUG操作后指令无误，不则, 不进行任何操作 
    }
    
    for i in {1..10}
    do
        DEBUG echo $i
    done
    
    可以将调用功能设置为 "on" 来运行上面的脚本 : 
    $ _DEBUG=on ./script.sh
    
只要在需要打印的语句前加上 DEBUG 。如果没有把 _DEBUG=on 传递给脚本, 那么调试信息就不会打印出来。在 Bash 中, 命令 ':' 告诉 shell 不要进行任何操作。


#### 补充内容 

还有其他脚本调试的便捷方法, 我们甚至可以巧妙地利用 shebang 来进行调试。

把shebang 从 `#!/bin/bash` 改成 `#!/bin/bash -xv`, 这样一来, 不用任何其他选项就可以启用调试功能了 : 

    #!/bin/bash -xv
    
    V1="Hello"
    V2="World"
    V3=$V1+$V2
    echo $V3
    
    输出 : 
    V1="Hello"
    + V1=Hello
    V2="World"
    + V2=World
    V3=$V1+$V2
    + V3=Hello+World
    echo $V3
    + echo Hello+World
    Hello+World