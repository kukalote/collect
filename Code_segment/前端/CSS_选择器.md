
## Css_选择器
### CSS 元素选择器
文档的元素就是最基本的选择器。
        
        html {color: green;}
        h1 {color: green;}
        h2 {color: green;}

### 类选择器
类选择器允许以一种独立于文档元素的方式来指定样式。

为了将类选择器的样式与元素关联，必须将 class 指定为一个适当的值。请看下面的 HTML 代码：
        
        .important { color: "red"}
        
        <h1 class="important">
        This heading is very important.
        </h1>
        <p class="important">
        This paragraph is very important.
        </p>
**结合元素选择器**  
        
        p.important {color:red;}
        //则只有 p 标签下的内容变为红色

### Css ID 选择器
ID 选择器允许以一种独立于文档元素的方式来指定样式。
**语法**

首先，ID 选择器前面有一个 # 号 - 也称为棋盘号或井号。

请看下面的规则：

        *#intro {font-weight:bold;}
        //*指任意标签下的 intro
与类选择器一样，ID 选择器中可以忽略通配选择器。前面的例子也可以写作：
        
        #intro {font-weight:bold;}
        
        <p id="intro">This is a paragraph of introduction.</p>
**Tips:**类选择器与 ID 选择器的区别 : 

**区别 1：只能在文档中使用一次**

与类不同，在一个 HTML 文档中，ID 选择器会使用一次，而且仅一次。
**区别 2：不能使用 ID 词列表**

不同于类选择器，ID 选择器不能结合使用，因为 ID 属性不允许有以空格分隔的词列表。
**区别 3：ID 能包含更多含义**

类似于类，可以独立于元素来选择 ID。有些情况下，您知道文档中会出现某个特定 ID 值，但是并不知道它会出现在哪个元素上，所以您想声明独立的 ID 选择器。例如，您可能知道在一个给定的文档中会有一个 ID 值为 mostImportant 的元素。您不知道这个最重要的东西是一个段落、一个短语、一个列表项还是一个小节标题。您只知道每个文档都会有这么一个最重要的内容，它可能在任何元素中，而且只能出现一个。在这种情况下，可以编写如下规则：

        #mostImportant {color:red; background:yellow;}

这个规则会与以下各个元素匹配（这些元素不能在同一个文档中同时出现，因为它们都有相同的 ID 值）：

        <h1 id="mostImportant">这样写是错误的</h1>
        <em id="mostImportant">这样写是错误的</em>
        <ul id="mostImportant">一个html文件中 ID 不能有相同值存在</ul>
**Tips:**ID 区分大小写
请注意，类选择器和 ID 选择器可能是区分大小写的。这取决于文档的语言。HTML 和 XHTML 将类和 ID 值定义为区分大小写，所以类和 ID 值的大小写必须与文档中的相应值匹配。

因此，对于以下的 CSS 和 HTML，元素不会变成粗体：

        #intro {font-weight:bold;}

        <p id="Intro">以上的样式对我没效果。</p>
### 分组选择器   
被分组的选择器就可以分享相同的声明。用逗号将需要分组的选择器分开。

        h1,h2,p,span#id,div.class_name {
            color: green;
        }
        
### 属性选择器
属性选择器可以根据元素的属性及属性值来选择元素。  
1. **简单属性选择**
        
        可以只对有 href 属性的锚（a 元素）应用样式：

        a[href] {color:red;}
        
        <a href="http://w3school.com.cn">我有 href 属性，字体为红色</a>
        <a name="w3school">我没有 href 属性，字体为默认颜色</a>
2. **根据具体属性值选择**
    
        a[title="W3School"] {color: red;}
        
        
        <a href="http://w3school.com.cn" title="aha">我有 title 属性, 但不为 W3School，字体为默认颜色</a>
        <a name="w3school" title="W3School">我有 title 属性，且为 W3School, 字体为红色</a>
3. **属性与属性值必须完全匹配**
如果属性值包含用空格分隔的值列表，匹配就可能出问题。  

        p[class="important warning"] {color: red;}
        <p class="important warning">This paragraph is a very important warning.</p>
如果写成 p[class="important"]，那么这个规则不能匹配示例标记。
4. **根据部分属性值选择 ** 
**还有一些其他的属性选择器看下表**

选择器 |	描述
----|---
`[attribute]` | 用于选取带有指定属性的元素。
`[attribute=value]` |	用于选取带有指定属性和值的元素。
`[attribute~=value]` | 用于选取属性值中包含指定词汇的元素。
`[attribute\|=value]` | 用于选取带有以指定值开头的属性值的元素，该值必须是整个单词。
`[attribute^=value]` | 匹配属性值以指定值开头的每个元素。
`[attribute$=value]` | 匹配属性值以指定值结尾的每个元素。
`[attribute*=value]` | 匹配属性值中包含指定值的每个元素。
### 派生选择器  
通过依据元素在其位置的上下文关系来定义样式，你可以使标记更加简洁。  
    
1. **CSS 后代选择器可以选择作为某元素后代的元素**(包括子元素、孙元素、重孙元素。。。)。    
    比方说，你希望列表中的 strong 元素变为斜体字，而不是通常的粗体字，可以这样定义一个派生选择器：  

        li strong {
            font-style: italic;
            font-weight: normal;
        }
        
    让我们来看一下这是什么意思，与其他选择器又有什么区别呢
        
        <strong>我是粗体字，不是斜体字，因为我不在列表当中，所以这个规则对我不起作用</strong>
        <ol>
            <li><strong>我是斜体字，我是子li标签的子元素。这是因为 strong 元素位于 li 元素内。</strong></li>
            <li><div><strong>虽然我不是li标签子元素，但是孙元素还是受到后代选择器的影响。</strong></div></li>
            <li>我是正常的字体。</li>
        </ol>
    之所以称为 后代 是因为无论经过几层，只要被父元素所包含，其下的便是子元素、孙元素等。  

2. **CSS 子元素选择器**  
   **如果您不希望选择任意的后代元素，而是希望缩小范围，只选择某个元素的子元素，请使用子元素选择器（Child selector）。**  
    格式：`父选择器>子选择器` (大于号前后的空格并不影响使用: h1 > strong)。    
    例如，如果您希望选择只作为 h1 元素子元素的 strong 元素，可以这样写：    

        h1 > strong {color:red;}

        <h1>这里有些 <strong>红色</strong> <strong>红色</strong> 的字.</h1>
        <h1>这里的字 <em>还是 <strong>默认颜色</strong></em> .</h1>
    **结合后代选择器和子选择器**  
    
        table.company td > p
        //上面的选择器会选择作为 td 元素子元素的所有 p 元素，这个 td 元素本身从 table 元素继承，该 table 元素有一个包含 company 的 class 属性。
    
    之所以称为 父子 是因为父元素会包含子元素,并且两层间无其他元素包含。  
3. **CSS 兄弟选择器**  
**相邻兄弟选择器（Adjacent sibling selector）可选择紧接在另一元素后的元素，且二者有相同父元素。**

        <div title="我是李家父亲" id="li">
            <b title="我是李家大儿子" id="li_1">
                div是我的父节点
            </b>
            <b title="我是李家二儿子" id="li_2">
                div是我的父节点
            </b>
            <b title="我是李家三儿子" id="li_3">
                div是我的父节点
            </b>
            <span title="我是李家四儿子" id="li_4">
                div是我的父节点
                <strong title="我是李家孙子" id="li_4_1">
                    div是我的祖父节点
                </strong>
            </span>
        </div>
这样我们就可以很清楚的了解到,`div是span、b标签的父节点`，`span又是strong标签的父节点`，那么既然都是一家人，`span、b当然就是兄弟节点`了
        
        b + span { color: "red"}
        //span 中的内容和 span 子元素 strong中的内容都变为红色，子元素有继承父元素样式的特性
**Tips:** 兄弟选择器只支持向后兄弟的查找，只能通过 #li_1 找到 #li_2，而不能反向通过 #li_2 指定 #li_1

        span + b { color: "red"}
        //#li_1 ,#li_2, #li_3 还是默认颜色
**Tips:** 兄弟选择器只能向后指定紧挨着的那个兄弟元素，
        
        b + b {color: "red"}
        //这下 #li_2, #li_3都是红色(#li_2 是 #li_1 的弟弟， #li_3 是 #li_2的弟弟) 
        //(若 span 为 #li_3, #li_4 为 b, 那么 #li_4 上的 b 不会成为红色)