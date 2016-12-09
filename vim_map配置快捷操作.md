### map简介

map是一个映射命令,将常用的很长的命令映射到一个新的功能键上。map是Vim强大的一个重要原因，可以自定义各种快捷键，用起来自然得心应手。
映射的种类


#### 有六种映射存在
> - 用于普通模式: 输入命令时。
- 用于可视模式: 可视区域高亮并输入命令时。
- 用于选择模式: 类似于可视模式，但键入的字符对选择区进行替换。
- 用于操作符等待模式: 操作符等待中 ("d"，"y"，"c" 等等之后)。
  见下: |omap-info|。
- 用于插入模式: 也用于替换模式。
- 用于命令行模式: 输入 ":" 或 "/" 命令时。

Command | Normal `(普通模式)` | Visual `(可视化模式)` | Operator Pending `(运算符模式)` | Insert Only `(插入模式)` | Command Line `(命令行模式)`
---|---|---|---|---|---
`:map` | Y | Y | Y | |
`:nmap` | Y | | | |
`:vmap` | | Y | | |
`:omap` | | | Y | |
`:map!` | | | | Y | Y
`:imap` | | | | Y |
`:cmap` | | | | | Y


#### 几种模式的介绍

1. **Normal Mode**
也就是最一般的普通模式，默认进入vim之后，处于这种模式。

2. **Visual Mode**
一般译作可视模式，在这种模式下选定一些字符、行、多列。
在普通模式下，可以按v进入。

3. **Insert Mode**
插入模式，其实就是指处在编辑输入的状态。普通模式下，可以按i进入。

4. **Select Mode**
选择模式。用鼠标拖选区域的时候，就进入了选择模式。和可视模式不同的是，在这个模式下，选择完了高亮区域后，敲任何按键就直接输入并替换选择的文本了。和windows下的编辑器选定编辑的效果一致。普通模式下，可以按gh进入。

5. **Command-Line/Ex Mode**
命令行模式和Ex模式。两者略有不同，普通模式下按冒号(:)进入Command-Line模式，可以输入各种命令，
使用vim的各种强大功能。普通模式下按Q进入Ex模式，其实就是多行的Command-Line模式。

#### 命令的组合

同Vim下的其他命令一样，命令的名字往往由好几段组成。前缀作为命令本身的修饰符，微调命令的效果。
对于map而言，可能有这么几种前缀：

> - **nore**
表示非递归。
递归的映射。其实很好理解，也就是如果键a被映射成了b，c又被映射成了a，如果映射是递归的，那么c就被映射成了b。
- **n**
表示在普通模式下生效
- **v**
表示在可视模式下生效
- **i**
表示在插入模式下生效
- **c**
表示在命令行模式下生效

#### 普通模式的映射命令

##### map

命令格式：
`:map {lhs} {rhs}`
其含义是，在:map作用的模式中把键系列 {lhs} 映射为 {rhs}，{rhs}可进行映射扫描，也就是可递归映射。

举例：
`:map td :tabnew .<cr>`
含义：在其作用模式（普通、可视、操作符）下，输入td等价于输入 `:tabnew . <cr>`。而普通模式下输入`:tabnew . <cr>`就是打开当前目录
如果再定义绑定 :map ts td，就是指在其作用模式下输入ts等价于td，也就是打开当前目录。不过如果没有特殊需要，一般不建议递归映射。

##### noremap

:noremap和:map命令相对，作用模式和命令格式都相同，只不过不允许再对{rhs}进行映射扫描，也就是{lhs}定义后的映射就是{rhs}的键序列，不会再对{rhs}键序列重新解释扫描。它一般用于重定义一个命令，当然如果:map不需要递归映射的话，建议使用:noremap
比如：
`:noremap ts td`
它的意思是在其作用模式下，输入ts就是输入td，但是和:map不同的是，此时td再不会做进一步扫描解释。虽然之前已经定义了td，但是不会对td再做扫描。

##### unmap

:unmap是对应取消:map绑定的｛lhs｝，作用模式相同，命令格式 :unmap {lhs}。
例如：
`:unmap td`
`:unmap <buffer> ,w`
就是取消在其作用模式中td的绑定，比如之前td被绑定为`:tabnew .<cr>`，此时此绑定消失。

##### mapclear

:mapclear时对应取消所有:map绑定的，慎用！
`:mapclear <buffer>`
当一个缓冲区被删除时局部映射也会被清除，但是在它被卸载时不会。就象局部选项值的
情况一样。

#### 只用于普通模式的

**:nmap**
:nmap是:map的普通模式板，也就是说其绑定的键只作用于普通模式。
例如：
`:nmap td :tabnew .<cr>` 和 `:map td :tabnew .<cr>` 在普通模式下等效

**:nnoremap**
:nnorempa 和 :nmap 的关系和 :noremap 和 :map 的关系一样，只是 :nmap 的非递归版

**:nunmap**
:nunmap和:nmap的关系和:unmap和:map的关系一样，取消:nmap的绑定。

**:nmapclear**
:nmapclear是对应取消所有:map绑定的，慎用！

另外

{rhs} 之前可能显示一个特殊字符:

    `*` 表示它不可重映射
    `&` 表示仅脚本的局部映射可以被重映射
    `@` 表示缓冲区的局部映射

到这一步你可以轻松的长吸一口气，因为相关的命令已经都了解了，记不住没关系，可以随时:help map一下。

键表 |key-notation|
> `<k0> - <k9>` 小键盘 0 到 9 `*keypad-0* *keypad-9*`
`<S-...>` `Shift＋键` `*shift* *<S-*`
`<C-...>` `Control＋键` `*control* *ctrl* *<C-*`
`<M-...>` `Alt＋键` 或 `meta＋键` `*meta* *alt* *<M-*`
`<A-...>` 同 `<m-...>` `*<A-*`
`<t_xx>` termcap 里的 "xx" 入口键

特殊参数：
1. `<buffer>`
2. `<silent>`
3. `<special>`
4. `<script>`
5. `<expr>`
6. `<unique>`
7. `<nowait>`

它们必须映射命令的后边，在其他任何参数的前面。

`<buffer>`如果这些映射命令的第一个参数是`<buffer>`，映射将只局限于当前缓冲区（也就是你此时正编辑的文件）内。比如：
`:map <buffer> ,w /a<CR>`
它的意思时在当前缓冲区里定义键绑定，“,w”将在当前缓冲区里查找字符a。同样你可以在其他缓冲区里定义：
`:map <buffer> ,w /b<CR>`
比如我经常打开多个标签(:tabedit)，想要在各自标签里定义",w"键绑定，那么你只要在每个标签页里分别定义就可，其作用域也只在各自的标签里。同样要清除这些缓冲区的键绑定也要加上`<buffer>`参数，比如：
`:unmap <buffer> ,w`
`:mapclear <buffer>`

`<silent>`是指执行键绑定时不在命令行上回显，比如：
`:map <silent> ,w /abcd<CR>`
你在输入,w查找abcd时，命令行上不会显示`/abcd`，如果没有`<silent>`参数就会显示出来

`<special>`一般用于定义特殊键怕有副作用的场合。比如：
`:map <special> <F12> /Header<CR>`
即使 'cpoptions' 包含了 "`<`" 标志位也没问题。这可用于不希望看到设置 'cpoptions' 时出现的副作用的场合。

`<script>` 如果给用于定义新映射或缩写的命令的第一个参数是 "`<script>`"，该映射只使用通过以 "`<SID>`" 开头来定义的的脚本局部映射来重映射 {rhs} 中的字符。这可以用于避免来自外部的脚本的干扰 (举例来说，在 mswin.vim 中 CTRL-V 被重新映射的时候就是如此)，但是又需要使用该脚本中定义的其它映射的情形。
备注: ":map `<script>`" 和 ":noremap `<script>`" 做同样的事情。这里 "`<script>`" 超越命令名。不过，更推荐使用 ":noremap `<script>`"，因为它更清晰地表示了重映射已被 (大多数时候) 禁止。

加入 `<nowait>` 参数。这样映射一旦匹配就会被使用，Vim 不会等待更多字符的输入。但如果那些字符已经输入了，还是会使用的。

`<unique>`一般用于定义新的键映射或者缩写命令的同时检查是否该键已经被映射，如果该映射或者缩写已经存在，则该命令会失败

`<expr>.` 如果定义新映射的第一个参数是`<expr>`，那么参数会作为表达式来进行计算，结果使用实际使用的`<rhs>`，例如：
`:inoremap <expr> . InsertDot()`
这可以用来检查光标之前的文本并在一定条件下启动全能 (omni) 补全。
一个例子：
```
	let counter = 0
	inoremap <expr> <C-L> ListItem()
	inoremap <expr> <C-R> ListReset()

	func ListItem()
	let g:counter += 1
	return g:counter . '. '
	endfunc

	func ListReset()
	let g:counter = 0
	return ''
	endfunc
```
在插入模式下，CTRL-L插入顺序的列表编号，并返回；CTRL-R复位列表编号到0，并返回空。


`<Leader> mapleader`
mapleader变量对所有map映射命令起效，它的作用是将参数`<leader>`替换成mapleader变量的值，比如：
`:map <Leader>A oanother line<Esc>`
如果mapleader变量没有设置，则用默认的反斜杠代替，因此这个映射等效于：
`:map \A oanother line<Esc>`
意思时输入\A键时，在下一行输入another line并返回到普通模式。
如果设置了mapleader变量，比如：
`let mapleader = ","`
那么就等效于：
`:map ,A oanother line<Esc>`

`<LocalLeader> maplocalleader`
`<LocalLeader>`和`<Leader>`类似，只不过它只作用于缓冲区。
因此在设置mapleader和maplocalleader时最好区分开，不要出现冲突。

大体上映射的主要部分已经都提到了，还有很多具体的映射相关的内容可以参见:help map
