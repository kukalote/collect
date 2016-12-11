### 获取终端信息

#### 获取终端的行数和列数

>tput cols  
tput lines

#### 打印终端名

> tput longname

#### 移动光标

> 移动光标到方位 (100, 100) 处  
> tput cup 100 100

#### 设置终端背景色

> tput setb no  
> 其中, `no` 可以在 0 到 7 之间取值

#### 设置文本样式

> tput bold
> 设置文本样式为粗体

#### 设置下划线的起止

> tput smul    
> tput rmul

#### 光标位置操作

> tput ed 删除当前光标位置到行尾的所有内容
> tput sc 存储光标位置  
> tput rc 恢复光标位置

    #!/bin/bash
    echo -n Count:
    tput sc
    
    count=0;
    while true;
    do
        if [ $count -lt 40 ];
        then 
            let count++;
            sleep 1;
            tput rc
            tput ed
            echo -n $count;
        else 
            exit 0;
        fi
    done



#### 输入密码时不显示输入

    #!/bin/bash
    echo -e "Enter password:"
    stty -echo    #不显示输入
    read password 
    stty echo     #再次显示
    echo          #暂停
    echo Password read.