####换行 : 加**两个空格**，或 **Ctrl + Return**

#### Headers

Setext-style:

This is H1
========

This is H2
----------

atx-style:

# This is H1
## This is H2
### This is H3
#### This is H4
##### This is H5
###### This is H6

####加粗 (Cmd + B): 
**加粗** __加粗__

####斜体 (CMD + I): 
*斜体* _斜体_

###引用 : 
>Simple inline link <http://chenluois.com>, another inline link Smaller, one more inline link with title Resize.

#### Links and Email
An email <example@example.com> link.

Simple inline link <http://chenluois.com>, another inline link [Smaller](http://25.io/smaller/), one more inline link with title [Resize](http://resizesafari.com "a Safari extension").

A [reference style][id] link. Input id, then anywhere in the doc, define the link with corresponding id:

[id]: http://25.io/mou/ "Markdown editor on Mac OS X"

Titles ( or called tool tips ) in the links are optional.

#### Images

An inline image ![Smaller icon](http://25.io/smaller/favicon.ico "Title here"), title is optional.

A ![Resize icon][2] reference style image.

[2]: http://resizesafari.com/favicon.ico "Title"

#### Inline code and Block code

Inline code are surround by `backtick` key. To create a block code:

	Indent each line by at least 1 tab, or 4 spaces.
	var Mou = exactlyTheAppIwant; 

####  Ordered Lists

有序队列可以使用 "1." + 空格:

1. Ordered list item
2. Ordered list item
3. Ordered list item

#### Unordered Lists

添加无序队列可以用 "*" + 空格:

* Unordered list item
* Unordered list item
* Unordered list item 

或者使用 "-" + 空格:

- Unordered list item
- Unordered list item
- Unordered list item

#### Hard Linebreak

End a line with two or more spaces will create a hard linebreak, called `<br />` in HTML. ( Control + Return )	
Above line ended with 2 spaces.  

#### Horizontal Rules

Three or more asterisks or dashes:

***

---

- - - -




### Extra Syntax

#### Footnotes 页脚注释

Footnotes work mostly like reference-style links. A footnote is made of two things: a marker in the text that will become a superscript number; a footnote definition that will be placed in a list of footnotes at the end of the document. A footnote looks like this:

That's some text with a footnote.[^1]

[^1]: And that's the footnote.


#### Strikethrough

Wrap with 2 tilde characters:

~~Strikethrough~~


#### Fenced Code Blocks

Start with a line containing 3 or more backticks, and ends with the first line with the same number of backticks:

```
Fenced code blocks are like Stardard Markdown’s regular code
blocks, except that they’re not indented and instead rely on
a start and end fence lines to delimit the code block.
```

####[Author](id:anchor1) 锚点操作
Click this [link](#anchor1) in the Preview view will auto scroll to the place of the destination anchor.


#### Tables

A simple table looks like this:

First Header | Second Header | Third Header
------------ | ------------- | ------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell

If you wish, you can add a leading and tailing pipe to each line of the table:

| First Header | Second Header | Third Header |
| ------------ | ------------- | ------------ |
| Content Cell | Content Cell  | Content Cell |
| Content Cell | Content Cell  | Content Cell |

Specify alignment for each column by adding colons to separator lines:

First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left		 | Center		| Right
Left		 | Center		| Right


### Shortcuts

#### View

* Toggle live preview: Shift + Cmd + I
* Toggle Words Counter: Shift + Cmd + W
* Toggle Transparent: Shift + Cmd + T
* Toggle Floating: Shift + Cmd + F
* Left/Right = 1/1: Cmd + 0
* Left/Right = 3/1: Cmd + +
* Left/Right = 1/3: Cmd + -
* Toggle Writing orientation: Cmd + L
* Toggle fullscreen: Control + Cmd + F

#### Actions

AA WORLD &lt;&gt;THIS HELLO

* Copy HTML: Option + Cmd + C
* Strong: 选中文本, Cmd + B
* Emphasize: 选中文本, Cmd + I
* Inline Code: 选中文本, Cmd + K
* Strikethrough: 选中文本, Cmd + U
* Link: 选中文本, Control + Shift + L
* Image: 选中文本, Control + Shift + I
* 选择单词: Control + Option + W
* 选择行: Shift + Cmd + L
* 全选: Cmd + A
* Deselect All: Cmd + D
* 转换为大写字母: 选中文本, Control + U
* 转换为小写字母: 选中文本, Control + Shift + U
* 转换为首字母大写: 选中文本, Control + Option + U
* Convert to List: Select lines, Control + L
* Convert to Blockquote: Select lines, Control + Q
* 转换成 H1: Cmd + 1
* 转换成 H2: Cmd + 2
* 转换成 H3: Cmd + 3
* 转换成 H4: Cmd + 4
* 转换成 H5: Cmd + 5
* 转换成 H6: Cmd + 6
* 将空格转为Tab: Control + [
* 将Tabs转换为空格: Control + ]
* Insert Current Date: Control + Shift + 1
* Insert Current Time: Control + Shift + 2
* 插入实体符 <: Control + Shift + ,
* 插入实体符 >: Control + Shift + .
* 插入实体符 &: Control + Shift + 7
* 插入实体符 Space: Control + Shift + Space
* Insert Scriptogr.am Header: Control + Shift + G 
* Shift Line Left: Select lines, Cmd + [
* Shift Line Right: Select lines, Cmd + ]
* 添加新行: Cmd + Return
* 添加评论: Cmd + /
* 强制换行: Control + Return

#### Edit

* Auto complete current word: Esc
* 查找: Cmd + F
* Close find bar: Esc

#### Post

* Post on Scriptogr.am: Control + Shift + S
* Post on Tumblr: Control + Shift + T

#### Export

* 导出为 HTML: Option + Cmd + E
* 导出为 PDF:  Option + Cmd + P


### And more?

Don't forget to check Preferences, lots of useful options are there.

Follow [@Mou](https://twitter.com/mou) on Twitter for the latest news.

For feedback, use the menu `Help` - `Send Feedback`