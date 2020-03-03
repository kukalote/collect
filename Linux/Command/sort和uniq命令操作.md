### 排序、唯一与重复

同文本文件打交道时, 少不了要用到排序。 `sort` 命令能够帮助我们对文本文件和 stdin 进行排序操作。它通常会配合其他命令来生成所需要的输出。 `uniq` 是一个经常与 `sort` 一同使用的命令。它的作用是从文本或 stdin 中提取唯一 (或重复) 的行。

#### sort

`sort` 命令即可以从特定的文件, 也可以从 stdin 中获取输入, 并将输出写入 stdout。

1. 对一组文件进行排序 : 

    	$ sort file1.txt file2.txt > sorted.txt
    	或
    	$ sort file1.txt file2.txt -o sorted.txt
    	
2. 按照数字顺序进行排序, -n : 

        $ sort -n file.txt
        
3. 按照逆序进行排序, -r : 

        $ sort -r file.txt
        
4. 按照月份进行排序 (依照一月, 二月,  三月…), -M : 

        $ sort -M months.txt
        
5. 合并两个已排序过的文件, -m  : 

        $ sort -m sorted1 sorted2

6. 找出已排序文件中不重复的行 :

        $ sort file1.txt file2.txt | uniq
        
7. 检查文件是否已经过排序 : 

        $ sort -C filename 
        $ echo $?
        
8. 用指定键排序 : 
    
        $ cat data
        1   mac 2000
        2   winxp   4000
        3   bsd 1000
        4   linux   1000
        
        # 依据第 1 列, 以逆序形式排序
        $ sort -nrk 1 data
        4   linux   1000
        3   bsd 1000
        2   winxp   4000
        1   mac 2000
        
        # 依据第 2 列进行排序
        $ sort -k 2 data
        3   bsd 1000
        4   linux   1000
        1   mac 2000
        2   winxp   4000
        
    通常在默认情况下, 键就是文本文件中的列。列与列之间用空格分隔。
    
    但有时, 我们需要将特定范围内的一组字符 (例如, key1=character4-character8) 作为键。这种情况下必须明确地将键指定为某个范围内的字符, 这个范围可以用键起止的字符位置来表明。  
    例如 : 
    
        $ cat data
        1010hellothis
        2189ababbba
        7464dfdddfdfd
        
        $ sort -nk 2,3 data
        1010hellothis
        2189ababbba
        7464dfdddfdfd 
        # 把醒目的字符作为数值键。为了rjbcp个键, 用字符在行内的起止位置作为键的书写格式(在上面的例子中, 起止位置是 2和3)。
        
        #用第一个字符作为键 : 
        $ sort -nk 1,1 data
        1010hellothis
        2189ababbba
        7464dfdddfdfd    

9. 排序后行尾无换行符, 以\0字符结尾, -z : 

    为了使 `sort` 的输出与以 `\0` 作为终止符的 `xargs` 命令相兼容, 采用下面的命令 : 
    
        $ sort -z data | xargs -0
        # 终止符 \0 用来保证 xargs 命令的使用安全

10. 忽略前导空格, 以字典序排序, -b -d : 
  
  有时文本中可能会包含一些像空格之类的不必要的多余字符。如果需要忽略这些字符, 并以字典序进行排序, 可以用 : 
    
        $ sort -bd unsorted.txt
        
    其中, 选项 `-b` 用于忽略文件中的前导空白行, 选项 `-d` 用于指明以字典序进行排序。




#### 结合 uniq 命令

`sort` 命令包含大量的选项, 能够对文件数据进行各种排序。如果使用 `uniq` 命令, 那 `sort` 更是必不可少, 因为前者要求输入数据必须经过排序。

要检查文件是否排序过, 可以采用以下方法 : 如果文件已经排序, `sort` 会返回为 0 的退出码 ($?), 否则返回非0。

`uniq` 登记通过消除重复内容, 从给定输入中 (stdin 或命令行参数文件) 找出唯一的行。它也可以用来找出输入中出现的重复行。

1. `uniq` 只能作用于排过序的数据输入, 因此, `uniq` 要么使用管道, 要么将排过序的文件作为输入, 与 `sort` 命令结合使用。
    
        $ cat sorted.txt
        bash
        foss
        hack
        hack
        
        $ uniq sorted.txt
        bash
        foss
        hack
        
        或是 
        
        $ sort unsorted.txt | uniq
        
2. 只显示唯一的行 (在输入文件中没有重复出现的行), -u : 
    
        $ uniq -u sorted.txt
        bash
        foss
        
        或是
        
        $ sort unsorted.txt | uniq -u
        
3. 要统计各行在文件中出现的次数, 使用下面的命令, -c : 
    
        $ sort unsorted.txt | uniq -c
            1 bash
            1 foss
            2 hack

4. 找出文件重复的行, -d : 

        $ sort unsorted.txt | uniq -d
        hack
        
5. 我们可以将 -s 结合 -w 来使用 : 

    > -s 指定可以跳过前 N 个字符;  
      -w 指定用于比较的最大字符数。
      
    这个对比键被用作 `uniq` 操作的索引 : 
    
        $ cat data
        u:01:gnu
        d:04:linux
        u:01:bash
        u:01:hack
        
    我们需要使用醒目的字符作为唯一的键。可以通过忽略前 2 个字符 (-s 2), 使用 -w 选项(-w 2)指定用于比较的最大字符数。
    
        $ sort data | uniq -s 2 -w 2
        d:04:linux
        u:01:bash    #现在在排序前面的被保留
        
6. 使用 \0 作定界符 : 

    我们将命令输入作为 `xargs` 命令的输入时, 最好为输出的各行添加一个 0 值字节终止符。在将 `uniq` 命令的输入作为 `xargs` 的数据源时, 同样应当如此。如果没有使用 0 值字节终止符, 那么在默认情况下, `xargs` 命令会用空格作为定界符分割参数。
    
    用 `uniq` 命令生成包含 0 值字节终止符的输出 : 
    
        $ uniq -z file.txt
        
    下面的命令将删除所有指定的文件, 这些文件的名字是从 file.txt 中读取的 : 
    
        $ uniq -z file.txt | xargs -0 rm
    
    如果某个文件名在文件中出现多次, `uniq` 命令只会将这个文件名写入 stdout 一次。
    
