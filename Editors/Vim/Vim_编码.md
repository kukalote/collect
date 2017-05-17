# Vim_编码

<!-- create time: 2016-03-31 09:03:53  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

## 1.相关基础知识介绍

在Vim中，有四个与编码有关的选项，它们是：`fileencodings`、`fileencoding`、`encoding`和`termencoding`。

### encoding

`encoding`是Vim内部使用的字符编码方式。当我们设置了`encoding`之后，Vim内部所有的buffer、寄存器、脚本中的字符串等，全都使用这个编码。Vim 在工作的时候，如果编码方式与它的内部编码不一致，它会先把编码转换成内部编码。如果工作用的编码中含有无法转换为内部编码的字符，在这些字符就会丢失。因此，在选择 Vim 的内部编码的时候，一定要使用一种表现能力足够强的编码，以免影响正常工作。

由于`encoding`选项涉及到Vim中所有字符的内部表示，因此只能在Vim启动的时候设置一次。在Vim工作过程中修改`encoding`会造成非常多的问题。用户手册上建议只在 .vimrc中改变它的值，事实上似乎也只有在 .vimrc中改变它的值才有意义。如果没有特别的理由，请始终将`encoding`设置为`utf-8`。为了避免在非`UTF-8`的系统如Windows下，菜单和系统提示出现乱码，可同时做这几项设置：

    set encoding=utf-8
    set langmenu=zh_CN.UTF-8
    language message zh_CN.UTF-8
    
### termencoding

`termencoding`是Vim用于屏幕显示的编码，在显示的时候，Vim会把内部编码转换为屏幕编码，再用于输出。内部编码中含有无法转换为屏幕编码的字符时，该字符会变成问号，但不会影响对它的编辑操作。如果`termencoding`没有设置，则直接使用`encoding`不进行转换。

举个例子，当你在Windows下通过telnet登录Linux工作站时，由于Windows的telnet是`GBK`编码的，而Linux下使用`UTF-8`编码，你在telnet下的Vim中就会乱码。此时有两种消除乱码的方式：一是把Vim的`encoding`改为gbk，另一种方法是保持encoding为`utf-8`，把termencoding改为gbk，让Vim在显示的时候转码。显然，使用前一种方法时，如果遇到编辑的文件中含有`GBK`无法表示的字符时，这些字符就会丢失。但如果使用后一种方法，虽然由于终端所限，这些字符无法显示，但在编辑过程中这些字符是不会丢失的。

对于图形界面下的GVim，它的显示不依赖TERM，因此`termencoding`对于它没有意义。在GTK2下的GVim 中，termencoding永远是utf-8，并且不能修改。而Windows下的GVim则忽略termencoding的存在。


### fileencodings

编码的自动识别是通过设置fileencodings实现的，注意是复数形式。fileencodings是一个用逗号分隔的列表，列表中的每一项是一种编码的名称。当我们打开文件的时候，VIM按顺序使用fileencodings中的编码进行尝试解码，如果成功的话，就使用该编码方式进行解码，并将fileencoding设置为这个值，如果失败的话，就继续试验下一个编码。

因此，我们在设置fileencodings的时候，一定要把要求严格的、当文件不是这个编码的时候更容易出现解码失败的编码方式放在前面，把宽松的编码方式放在后面。例如，latin1是一种非常宽松的编码方式，任何一种编码方式得到的文本，用latin1进行解码，都不会发生解码失败——当然，解码得到的结果自然也就是理所当然的“乱码”。因此，如果你把latin1放到了fileencodings的第一位的话，打开任何中文文件都是乱码也就是理所当然的了。

以下是网上推荐的一个fileencodings设置：

    set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1

其中，ucs-bom是一种非常严格的编码，非该编码的文件几乎没有可能被误判为ucs-bom，因此放在第一位。
`utf-8`也相当严格，除了很短的文件外(例如许多人津津乐道的`GBK`编码的“联通”被误判为`UTF-8`编码的经典错误)，现实生活中一般文件是几乎不可能被误判的，因此放在第二位。

接下来是`cp936`和`gb18030`，这两种编码相对宽松，如果放前面的话，会出现大量误判，所以就让它们靠后一些。`cp936`的编码空间比`gb18030`小，所以把`cp936`放在`gb18030`前面。

至于`big5`、`euc-jp`和`euc-kr`，它们的严格程度和`cp936`差不多，把它们放在后面，在编辑这些编码的文件的时候必然出现大量误判，但这是Vim内置编码探测机制没有办法解决的事。由于中国用户很少有机会编辑这些编码的文件，因此我们还是决定把`cp936`和`gb18030`放在前面以保证这些编码的识别。

最后就是`latin1`了。它是一种极其宽松的编码，以至于我们不得不把它放在最后一位。不过可惜的是，当你碰到一个真的`latin1`编码的文件时，绝大部分情况下，它没有机会fall-back到`latin1`，往往在前面的编码中就被误判了。不过，正如前面所说的，中国用户没有太多机会接触这样的文件。

如果编码被误判了，解码后的结果就无法被人类识别，于是我们就说，这个文件乱码了。此时，如果你知道这个文件的正确编码的话，可以在打开文件的时候使用 `++enc=encoding` 的方式来打开文件，如：

    :e ++enc=utf-8 myfile.txt
    
    
    
## 2.Vim的工作原理

好了，解释完了这一堆容易让新手犯糊涂的参数，我们来看看Vim的多字符编码方式支持是如何工作的。

1. Vim启动，根据 .vimrc中设置的`encoding`的值来设置buffer、菜单文本、消息文的字符编码方式。
2. 
2. 读取需要编辑的文件，根据`fileencodings`中列出的字符编码方式逐一探测该文件编码方式。并设置`fileencoding`为探测到的，看起来是正确的字符编码方式。事实上，Vim 的探测准确度并不高，尤其是在`encoding`没有设置为`utf-8`时。因此强烈建议将`encoding`设置为`utf-8`，虽然如果你想Vim显示中文菜单和提示消息的话这样会带来另一个小问题。

3. 对比`fileencoding`和`encoding`的值，若不同则调用iconv将文件内容转换为`encoding`所描述的字符编码方式，并且把转换后的内容放到为此文件开辟的buffer里，此时我们就可以开始编辑这个文件了。注意，完成这一步动作需要调用外部的`iconv.dll`(注2)，你需要保证这个文件存在于`$VIMRUNTIME`或者其他列在`PATH`环境变量中的目录里。

4. 编辑完成后保存文件时，再次对比`fileencoding`和`encoding`的值。若不同，再次调用iconv将即将保存的buffer中的文本转换为`fileencoding`所描述的字符编码方式，并保存到指定的文件中。同样，这需要调用`iconv.dll`


## 3.解决办法示例

（1）方法一：设定.vimrc文件：

在`/home/username/.vimrc`或者`/root/.vimrc`下增加两句话：

    let &termencoding=&encoding
    set fileencodings=utf-8,gbk,ucs-bom,cp936

这种办法可以实现编辑UTF-8文件

（2）方法而二：打开文件后，在vi编辑器中设定：

    :set encoding=utf-8 termencoding=gbk fileencoding=utf-8

（3）方法三：新建UTF-8文件，在vi编辑器设定：

    :set fenc=utf-8
    :set enc=GB2312

这样在编辑器里输入中文，保存的文件是UTF-8。

（4）方法四：一个推荐的～/.vimrc文件配置：
    
    set encoding=utf-8
    set fileencodings=ucs-bom,utf-8,cp936,gb18030,latin1
    set termencoding=gb18030
    set expandtab
    set ts=4
    set shiftwidth=4
    set nu
    syntax on
    
    if has('mouse')
    set mouse-=a
    endif 