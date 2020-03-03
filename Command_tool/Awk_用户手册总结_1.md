# Awk 用户手册总结_1

### 使用 awk 编程

awk 程序有几种运行方式：
```bash
$ awk 'program' input-file1 input-file2 …
$ awk -f program-file input-file1 input-file2 …
```
这里不再详细介绍命令行执行方式，主要说一下 awk 命令脚本方式：
```bash
$ cat advice.awk
>#! /bin/awk -f
>BEGIN { print "Don't Panic!" }
$ chmod +x advice.awk
$ ./advice.awk
>Don't Panic!
```
> Tips : 在 awk 脚本文件中，我们可以直接使用单引号。

### awk 中的注释

在 awk 脚本中，可以使用井号注释后面的行，当然在命令行也可以使用，但需要谨慎使用 ：
```bash
# This program prints a nice, friendly message.  It helps
# keep novice users from being afraid of the computer.
BEGIN    { print "Don't Panic!" }
```
```bash
# 虽然可以使用，但像下面这样会提示异常的。因为 # 会把后面的闭合单引号也会被注释掉
$ awk 'BEGIN { print "hello" } # let's be cute'
```

### awk 与引号

运行 awk 程序可以使用单引号:
```bash
$ awk `program`
```
> Tips :  在循环输入/输出型命令中，需要使用 Ctrl-d 来终止。

在单引号的命令中输出带单引号的字符使用方法 ：
```bash
$ awk 'BEGIN { print "Don\47t Panic!" }'
> Don't Panic!
```
> Tips ：这里我们可以使用 `\47` 来替换单引号。

双引号也可以在这里使用 ：
```bash
$ awk "BEGIN { print \"Don't Panic!\" }"
> Don't Panic!
```
> Tips : 有些特殊字符，我们需要使用 反引号 `\` 来进行转义， 如 : `$`, `\`,`"`,**`**

1. 混合使用单引号和双引号是件很麻烦的事，像这样 ：
```bash
$ awk 'BEGIN { print "Here is a single quote <'"'"'>" }'
> Here is a single quote <'>
```
也可以由三段组装起来，第一，二段用单引号，第二段用的是双引号 ：
```bash
$ awk 'BEGIN { print "Here is a single quote <'\''>" }'
> Here is a single quote <'>
```
2. 另一种选择是双引号 :
```bash
$ awk "BEGIN { print \"Here is a single quote <'>\" }"
> Here is a single quote <'>
```
3. 再就是使用编码方式表达,这样使用效果不错，但需要注释清楚 ：
```bash
$ awk 'BEGIN { print "Here is a single quote <\47>" }'
> Here is a single quote <'>
$ awk 'BEGIN { print "Here is a double quote <\42>" }'
> Here is a double quote <">
```
4. 最后介绍使用变量的方式 ：
```bash
$ awk -v sq="'" 'BEGIN { print "Here is a single quote <" sq ">" }'
> Here is a single quote <'>
```
### 下面介绍一些操作例子

使用两个测试数据文件 ：
**mail-list **文件，字段分别为 名字，电话，邮箱，人物类型(A：认识，F：朋友，R：亲戚)
```
Amelia       555-5553     amelia.zodiacusque@gmail.com    F
Anthony      555-3412     anthony.asserturo@hotmail.com   A
Becky        555-7685     becky.algebrarum@gmail.com      A
Bill         555-1675     bill.drowning@hotmail.com       A
Broderick    555-0542     broderick.aliquotiens@yahoo.com R
Camilla      555-2912     camilla.infusarum@skynet.be     R
Fabius       555-1234     fabius.undevicesimus@ucb.edu    F
Julie        555-6699     julie.perscrutabor@skeeve.com   F
Martin       555-6480     martin.codicibus@hotmail.com    A
Samuel       555-3430     samuel.lanceolis@shu.edu        A
Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
```

**inventory-shipped** 文件，字段为 月份，绿，红，橙，蓝货箱的出货量
```
Jan  13  25  15 115
Feb  15  32  24 226
Mar  15  24  34 228
Apr  31  52  63 420
May  16  34  29 208
Jun  31  42  75 492
Jul  24  34  67 436
Aug  15  34  47 316
Sep  13  55  37 277
Oct  29  54  68 525
Nov  20  87  82 577
Dec  17  35  61 401

Jan  21  36  64 620
Feb  26  58  80 652
Mar  24  75  70 495
Apr  21  70  74 514
```

例子 ：
1. 显示匹配行
```bash
$ awk '/li/ { print $0 }' mail-list
```
2. 打印长度超过 80 个字的行
```bash
$ awk 'length($0) > 80' data
```
3. 打印最长的行，字符数
```bash
$awk '{ if (length($0) > max) max = length($0) }
     END { print max }' data
```
4. 打印最长的行
```bash
$ awk '{ if (length($0) > max) {max = length($0); max_line = $0} }
     END { print max_line }' /tmp/data
```
5. 打印有字段的行
```bash
$ awk 'NF > 0' data
```
> Tip : 这个功能用来删除空行很有效
6. 打印 7 个0-100的随机数
```bash
$ awk 'BEGIN { for (i = 1; i <= 7; i++)
                 print int(101 * rand()) }'
```
7. 打印目录下文件总容量 
```bash
$ ls -l files | awk '{ x += $5 }
                   END { print "total bytes: " x }'
```
8. 打印目录下文件总容量，以KB表示
```bash
$ ls -l files | awk '{ x += $5 }
   END { print "total K-bytes:", x / 1024 }'

```
9. 打印经过排序的登陆用户名
```bash
$ awk -F: '{ print $1 }' /etc/passwd | sort
```
10. 统计文件行数
```bash
$ awk 'END { print NR }' data
```
11. 打印单数行内容
```bash
$ awk 'NR % 2 == 0' data
```

### 使用多重规则

下面将介绍，如何在 awk 程序中，使用多重过滤规则和多个文件

```bash
$ $ awk '/12/ { print $0 }
>      /21/ { print $0 }' mail-list inventory-shipped
>  Anthony      555-3412     anthony.asserturo@hotmail.com   A
>  Camilla      555-2912     camilla.infusarum@skynet.be     R
>  Fabius       555-1234     fabius.undevicesimus@ucb.edu    F
>  Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
>  Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
>  Jan  21  36  64 620
>  Apr  21  70  74 514
```
> Tip : 如上例，如果某行数据分别匹配多条规则，也会输出多条匹配行。

### awk 声明与换行

在一些声明中，我们可以用 **换行** 替换 **;**, 或者单独换行。不过，gawk 将会忽略以下字符或关键字后的换行 : `,    {    ?    :    ||    &&    do    else`。

如果确实需要换行，需要在换行符前加上 反斜杠(`\`),保留换行符。

