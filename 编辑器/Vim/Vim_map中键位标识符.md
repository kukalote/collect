下面这些按键的名称文档里会用到。它们也可以用在 "`:map`" 命令里 (按下 `CTRL-K` 再按
下你想输入的键就可以输入该键的键名)。

标识符 | 含义 | 等价于 |十进制数值
----|----|----|----
`<Nul>` | 零 |`CTRL-@`|0 (存储为 10)
`<BS>` | 退格键 | `CTRL-H` |8
`<Tab>` | 制表符 | `CTRL-I` |9
`<NL>` | 换行符 | `CTRL-J` | 10 (用作 `<Nul>`)
`<FF>` | 换页符 | `CTRL-L` | 12
`<CR>` | 回车符 | `CTRL-M` | 13
`<Return>` | 同 `<CR>`  | |
`<Enter>` | 同 `<CR>`  | |
`<Esc>` | 转义 | `CTRL-[` |27
`<Space>` | 空格  | | 32
`<lt>` | 小于号 | `<` | 60
`<Bslash>` | 反斜杠 | `\` | 92
`<Bar>` | 竖杠 | | 124
`<Del>` | 删除  | | 127
`<CSI>` | 命令序列引入 | `ALT-Esc` | 155
`<xCSI>` | 图形界面的 CSI  | |
`<EOL>` | 行尾 (可以是 `<CR>`、`<LF>` 或 `<CR><LF>`， 根据不同的系统和 'fileformat' 而定)  | |
`<Up>` | 光标上移键  | |
`<Down>` | 光标下移键  | |
`<Left>` | 光标左移键  | |
`<Right>` | 光标右移键  | |
`<S-Up>` | `Shift＋光标上移键`  | |
`<S-Down>` | `Shift＋光标下移键`  | |
`<S-Left>` | `Shift＋光标左移键`  | |
`<S-Right>` | `Shift＋光标右移键`  | |
`<C-Left>` | `Control＋光标左移键`  | |
`<C-Right>` | `Control＋光标右移键`  | |
`<F1> - <F12>` | `功能键 1 到 12`  | |
`<S-F1> - <S-F12>` | `Shift＋功能键 1 到 12`  | |
`<Help>` | 帮助键  | |
`<Undo>` | 撤销键  | |
`<Insert>` | `Insert 键`  | |
`<Home>` | Home  | |
`<End>` | End  | |
`<PageUp>` | `Page-up`  | |
`<PageDown>` | `Page-down`  | |
`<kHome>` | `小键盘 Home (左上)`  | |
`<kEnd>` | `小键盘 End (左下)`  | |
`<kPageUp>` | `小键盘 Page-up (右上)`  | |
`<kPageDown>` | `小键盘 Page-down (右下)`  | |
`<kPlus>` | `小键盘 +`  | |
`<kMinus>` | `小键盘 -`  | |
`<kMultiply>` | `小键盘 *`  | |
`<kDivide>` | `小键盘 /`  | |
`<kEnter>` | `小键盘 enter`  | |
`<kPoint>` | `小键盘 小数点`  | |
`<k0> - <k9>` | `小键盘 0 到 9`  | |
`<S-...>` | `Shift＋键`  | |
`<C-...>` | `Control＋键`  | |
`<M-...>` | `Alt＋键` 或 `meta＋键`  | |
`<A-...>` | 同 `<m-...>`  | |
`<D-...>` | `Command＋键` (只用于苹果机)  | |
`<t_xx>` | termcap 里的 "xx" 入口键  | |
