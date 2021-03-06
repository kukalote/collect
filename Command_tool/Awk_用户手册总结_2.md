# Awk 用户手册总结_2

awk 一些命令行选项：
**`-F fs`**
**`--field-separator fs`** 
> 设置分隔符，默认为空格

**`-f source-file`**
**`--file source-file`**
> 从 source-file 读取 awk 程序，此选项可多次调用 ，awk 程序将把多次调用的 source-file 组合起来调用。

**`-v var=val`**
**`--assign var=val`**
> 设置 var 变量，此项将在 BEGIN 字段之前生效。再就是， `-v` 只能声明一个变量，不过你可以多次调用。使用的时候 awk 如果需要会重置变量的值，可能会忽略你给的初始值。

**`-W gawk-opt`**
> 提供一个实现特定的选项

**`--`**
> 标记命令行选项结尾。此后的参数将不作为选项处理，常用来处理后面脚本名或文件名中包含 `-` 的情况。

**`-b`**
**`--characters-as-bytes`**
> 由于 gawk 会将所有输入数据都做为单字节词对待，另外，所有以 print 或 printf 的输出也被当作单字节词对待。
通常， gawk 按照 POSIX 标准并且试图以当前状态去处理输入数据。假如输入数据不包含有效的多字节字符，这常常会引入转换多字节为宽字节引出的问题和异常。这个选项以更简单的方式告诉 gawk, 告别我的数据。

**`-c`**
**`--traditional`**
> 在 awk 语言失效时，使用 兼容模式

**`-C`**
**`--copyright`**
> 打印版本许可等信息。

**`-d[file]`**
**`--dump-variables[=file]`**
> 打印经过排序的全局变量，它们的类型和最后值到指定文件。如果没有提供文件名，会在当前目录生成一个 **awkvars.out**。`-d`和文件名之间没有空格。

**`-D[file]`**
**`--debug[=file]`**
> 可以调试 awk 程序。通常，debugger 从键盘读取输入，但 file 参数允许你从指定文件中读取一系列的命令。在 `-D` 和 `file` 中间没有空格。

**`-e program-text`**
**`--source program-text`**
> program-text 部分为程序源码。这个选项让你可以在命令行输入程序源码。一般用在命令行中调用库函数的情况。
```bash
$ gawk -e 'BEGIN { a = 5 ;' -e 'print a }'
> 5
```

**`-E file`**
**`--exec file`**
> 与 `-f` 选项类似，从文件中读取 awk 程序，但有如下两点区别 :
1. 该选项终止选项处理;命令行上的其他内容直接传递给awk程序;
2. 命令行中，以 'var=value' 声明的变量赋值无法使用。
对于通过URL传递参数的万维网CGI应用程序，此选项尤其有必要。使用此选项可防止恶意（或其他）用户将选项，分配或awk源代码（通过-e）传递给CGI应用程序.此选项应与'`＃！`'脚本一起使用 ：
```bash
#! /usr/local/bin/gawk -E
awk program here …
```

**`-g`**
**`--gen-pot`**
> 分析源程序并且生成一个标准输出的模板文件，用于已标记为翻译的所有字符串常量。

**`-h`**
**`--help`**
> 输出帮助信息

**`-i source-file`**
**`--include source-file`**
> 从 source-file 中导入一个 awk 源代码库，就像在代码中使用 @include 指令一样。虽然有些像 `-f` 指令，但有两个不同之处：
1. 当使用 `-i` 指令时，不会重复加载文件，不同的 `-f` 每次都会加载;
2. 由于此选项旨在与代码库一起使用，因此gawk不会将这些文件识别为构成主程序输入。因此，在处理完 `-i` 参数后，gawk仍然希望通过 `-f` 选项或在命令行上找到主要的源代码。

**`-l ext`**
**`--load ext`**
> 加载 ext 动态扩展。这个选项将以 **AWKLIBPATH** 为环境变量搜索库文件。类似程序中用到的 @load 关键字。

**`-L[value]`**
**`--lint[=value]`**
> 