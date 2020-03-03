# Shell 颜色和闪烁控制

引自：http://www.jianshu.com/p/ba1b8aded634

> 在Shell下有时候需要定制输出，比如给输出加上颜色，或者显示高亮，或者添加闪烁等。
 然后这些颜色代码或者控制码等相对不好记住。这个时候我们可以考虑把最终想要的结果制定成对应的函数，
 在使用的时候直接调用函数会方便很多

## 格式

```bash
echo -e "\033[字背景颜色;字体颜色m字符串\033[控制码"
```

## 定制颜色函数

```bash
## blue to echo 
function blue(){
    echo -e "\033[34m[ $1 ]\033[0m"
}


## green to echo 
function green(){
    echo -e "\033[32m[ $1 ]\033[0m"
}

## Error to warning with blink
function bred(){
    echo -e "\033[31m\033[01m\033[05m[ $1 ]\033[0m"
}

## Error to warning with blink
function byellow(){
    echo -e "\033[33m\033[01m\033[05m[ $1 ]\033[0m"
}


## Error
function bred(){
    echo -e "\033[31m\033[01m[ $1 ]\033[0m"
}

## warning
function byellow(){
    echo -e "\033[33m\033[01m[ $1 ]\033[0m"
}
```

可以把这些函数写入到一个公共的SHELL脚本中，每次在编写其他脚本的时候用如下方式调用，即可

```bash
source /root/bin/common
```

或者可以把上述代码直接粘贴到当前编写的脚本中去。

当然可以推荐第一种方式。不用每次都复制粘贴。 直接 **source 调用** 即可
举例

```bash
root@pts/4 $ cat /root/bin/common 
#!/usr/bin/env bash

## blue to echo 
function blue(){
    echo -e "\033[35m[ $1 ]\033[0m"
}


## green to echo 
function green(){
    echo -e "\033[32m[ $1 ]\033[0m"
}

## Error to warning with blink
function bred(){
    echo -e "\033[31m\033[01m\033[05m[ $1 ]\033[0m"
}

## Error to warning with blink
function byellow(){
    echo -e "\033[33m\033[01m\033[05m[ $1 ]\033[0m"
}


## Error
function red(){
    echo -e "\033[31m\033[01m[ $1 ]\033[0m"
}

## warning
function yellow(){
    echo -e "\033[33m\033[01m[ $1 ]\033[0m"
}
Dev-web-solr [/opt/hexo2] 2016-11-28 17:52:03
root@pts/4 $ cat /root/bin/test.sh 
#!/usr/bin/env bash

source /root/bin/common

green "hello world with green color"
blue "hello world with blue color"

bred "error info with blink"
byellow "warning info with blink"
```
附加 shell输出 字体背景颜色和字体颜色，控制码等参数
## 字体背景颜色

属性性码 | 颜色
---|---
40|黑
41|深红
42|绿
43|黄色
44|蓝色
45|紫色
46|深绿
47|白色

## 字体颜色

属性性码 | 颜色
---|---
30|黑
31|红
32|绿
33|黄
34|蓝色
35|紫色
36|深绿
37|白色

## 控制码

这里常用有 设置高亮度/下划线/闪烁/关闭所有属性

控制码 | 属性
---|---
`\33[0m` | 关闭所有属性
`\33[01m` | 设置高亮度
`\33[04m` | 下划线
`\33[05m` | 闪烁
`\33[07m` | 反显
`\33[08m` | 消隐
`\33[30m -- \33[37m` | 设置前景色
`\33[40m -- \33[47m` | 设置背景色
`\33[nA` | 光标上移n行
`\33[nB` | 光标下移n行
`\33[nC` | 光标右移n行
`\33[nD` | 光标左移n行
`\33[y;xH`| 设置光标位置
`\33[2J` | 清屏
`\33[K` | 清除从光标到行尾的内容
`\33[s` | 保存光标位置
`\33[u` | 恢复光标位置
`\33[?25l` | 隐藏光标
`\33[?25h` | 显示光标
