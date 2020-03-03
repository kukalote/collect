### 录制与回放终端会话

当你需要在别人终端上演示某些操作或是需要准备一个命令教程时, 我们可以将命令及结果按先后次序记录下来, 再进行回放, 从而达到传输和教学的目的。

1. `script` 和 `scriptreplay` 命令在绝大数系统上都可以找到。把终端会话记录到一个文件中, 然后可以与其他人分享。

2. 开始录制终端会话 : 

        $ script -t 2> timing.log -a output.session
        type commands;
        …
        …
        exit
        
    两个配置文件被当做 `script` 命令的参数。其中一个文件 (timing.log) 用于存储时序信息, 描述每一个命令在何时运行; 另一个文件 (output.session) 用于存储命令输出。 `-t` 选项用于将时序数据导入 stderr。2> 则用于将 stderr 重定向到 timing.log。
    
    借助这两个文件 : timing.log (存储时序信息) 和 output.session (存储命令输入信息), 我们可以按照下面的方法回放命令执行过程 : 
    
        $ scriptreplay timing.log output.session # 按播放命令序列输出
        
#### 工作原理

通常我们会录制桌面环境视频来作为教程使用。不过要注意的是, 视频需要大量的存储空间, 而终端脚本仅仅是一个文本文件, 其大小不过是KB级别。

`script` 命令同样可以用于建立可在多个用户之间进行广播的视频会话。

打开两个终端 terminal1 和 terminal2

1. 在 terminal1 中输入以下命令 : 

        $ mkfifo scriptfifo
        
2. 在 terminal2 中输入以下命令 : 

        $ cat scriptfifo
        
3. 返回 terminal1 , 输入以下命令

        $ script -f scriptfifo    #本人使用 mac电脑命令为 "script -a scriptfifo"
        $ command;
        
4. 如果要结束会话的话, 输入 exit 并按回车键。

现在 terimnal1 成了广播员, 它会在 terminal2 中进行实时播放了。