### Linux cmd comm_文本文件的交集与差集

`comm` 命令可用于两个文件之间的比较。它有很多不错的选项可用来调整输出, 以便我们执行交集(intersection)、求差(difference)以及差集(set difference)操作。

- **交集** : 打印出两个文件所共有的行。
- **求差** : 打印出指定文件所包含的且互不相同的那些行。
- **差集** : 打印出包含在文件A中, 但不包含在其他指定的文件中的那些行。

#### 实战演练

需要注意的是 `comm` 必须使用排过序的文件作为输入。

    $ cat A.txt
    apple
    orange
    gold
    silver
    steel
    iron
    
    $ cat B.txt
    orange
    gold
    cookies
    carrot
    
    $ sort A.txt -o A.txt; sort B.txt -o B.txt
    
1. 首先不带参数的 comm 命令 : 
    
        $ comm A.txt B.txt
        apple
        	carrot
        	cookies
        		gold
        iron
        		orange
        silver
        steel
    
输出的第一列包含只在 A.txt 中出现的行, 第二列包含只在 B.txt 中出现的行, 第三列包含 A.txt 和 B.txt 中相同的行。各列以制表符 (\t) 作为定界符。

2. 为了打印两个文件的交集, 我们需要删除第一列和第二列, 只打印出第三列 : 

        $ comm A.txt B.txt -1 -2
        gold 
        orange
    
3. 打印出两信文件中不同的行

        $ comm A.txt B.txt -3
        apple
        	carrot
        	cookies
        iron
        silver
        steel
    
在这次的输出中, 那些唯一出现的行使得列中出现了空白字段。所以这两列在同一行上不会同时都出现内容。为了提高输出结果的可用性, 需要删除空白字段, 将两列合并成一列 : 

4. 要生成规范的输出, 得使用下面的命令 : 
    
        $ comm A.txt B.txt -3 | sed 's/^\t//'
        apple
    	carrot
    	cookies
        iron
        silver
        steel
        
5. 通过删除不需要的列, 我们就可以分别得到 A.txt 和 B.txt 的差集。

    - A.txt 的差集
        
            $ comm A.txt B.txt -2 -3
            # -2 -3 删除第二列和第三列
            
    - B.txt 的差集
    
            $ comm A.txt B.txt -1 -3 
            #-1 -3 删除第一列和第三列
            
#### 工作原理

`comm` 的命令行选项可以按照需要对输出进行格式化, 

- `-1` 从输出中删除第一列;
- `-2` 从输出中删除第二列;
- `-3` 从输出中删除第三列;

输出中的行首有的会用 `\t` 字符, 可以用 `sed 's/^\t//'` 进行删除。

 
   
    