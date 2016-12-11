### HTML_表格

**表格标签列表**

标签 | 描述
----|---
[`<table>`](#table) | 定义表格
[`<caption>`](#caption) | 定义表格标题。
[`<th>`](#th) | 定义表格中的表头单元格。
[`<tr>`](#tr) | 定义表格中的行。
[`<td>`](#td) | 定义表格中的单元。
[`<thead>`](#thead) | 定义表格中的表头内容。
[`<tbody>`](#tbody) | 定义表格中的主体内容。
[`<tfoot>`](#tfoot) | 定义表格中的表注内容（脚注）。
[`<col>`](#col) | 定义表格中一个或多个列的属性值。
[`<colgroup>`](#colgroup) | 定义表格中供格式化的列组。


#### [HTML &lt;table&gt; 标签](id:table)
**定义和用法 : **

`<table>` 标签定义 HTML 表格。  
简单的 HTML 表格由 table 元素以及一个或多个 tr、th 或 td 元素组成。  
tr 元素定义表格行，th 元素定义表头，td 元素定义表格单元。  

**元素的属性 : **  

属性 | 值 | 描述
----|---|---
align | left,  center, right | **不赞成使用。**请使用样式代替。 规定表格相对周围元素的对齐方式。
bgcolor | rgb(x,x,x),  #xxxxxx,   colorname | **不赞成使用。**请使用样式代替。 规定表格的背景颜色。
border | pixels | 规定表格边框的宽度。(***)
cellpadding | pixels % | 规定单元边沿与其内容之间的空白。(***)
cellspacing | pixels % | 规定单元格之间的空白。(***)
frame | 看[frame](#frame)属性说明 | 规定外侧边框的哪个部分是可见的。
rules | 看[rules](#rules)属性说明 | 规定内侧边框的哪个部分是可见的。
summary | text | 规定表格的摘要。
width |  pixels % |  规定表格的宽度。(**)

**[frame属性说明](id:frame)**

值 | 描述
----|---
void | 不显示外侧边框。
above | 显示上部的外侧边框。
below | 显示下部的外侧边框。
hsides | 显示上部和下部的外侧边框。
vsides | 显示左边和右边的外侧边框。
lhs | 显示左边的外侧边框。
rhs | 显示右边的外侧边框。
box | 在所有四个边上显示外侧边框。
border | 在所有四个边上显示外侧边框。

**例子 : **

1. Table 的 frame="box"

        <table frame="box">
          <tr>
            <th>Month</th>
            <th>Savings</th>
          </tr>
          <tr>
            <td>January</td>
            <td>$100</td>
          </tr>
        </table>
    <table frame="box">
        <tr>
            <th>Month</th>
            <th>Savings</th>
        </tr>
        <tr>
            <td>January</td>
            <td>$100</td>
        </tr>
    </table>

2. Table 的 frame="above"

        <table frame="above">
          <tr>
            <th>Month</th>
            <th>Savings</th>
          </tr>
          <tr>
            <td>January</td>
            <td>$100</td>
          </tr>
        </table>
    <table frame="above">
        <tr>
            <th>Month</th>
            <th>Savings</th>
        </tr>
        <tr>
            <td>January</td>
            <td>$100</td>
        </tr>
    </table>
3. Table 的 frame="void"

        <table frame="void">
          <tr>
            <th>Month</th>
            <th>Savings</th>
          </tr>
          <tr>
            <td>January</td>
            <td>$100</td>
          </tr>
        </table>
    <table frame="void">
        <tr>
            <th>Month</th>
            <th>Savings</th>
        </tr>
        <tr>
            <td>January</td>
            <td>$100</td>
        </tr>
    </table>
    
**[rules属性说明](id:rules) : **  
rules 属性规定内侧边框的哪个部分是可见的。  
从实用角度出发，最好不要规定 rules，而是使用 CSS 来添加边框样式。

        rules 属性在 Firefox 和 Opera 中可以正确地显示。
        Internet Explorer、Chrome 以及 Safari 3 对该属性的显示并不正确。
    
        IE: 除了内侧边框，还会添加 4 边的外侧边框。
        Chrome 和 Safari：除了内侧边框，还会添加 affected 外侧边框。

    值 | 描述
----|---
none | 没有线条。
groups | 位于行组和列组之间的线条。
rows | 位于行之间的线条。
cols | 位于列之间的线条。
all | 位于行和列之间的线条。
**例子 : **

        <table rules="rows" summary="这是表格摘要">
          <tr>
            <th>Month</th>
            <th>Savings</th>
          </tr>
          <tr>
            <td>January</td>
            <td>$100</td>
          </tr>
        </table>

    <table rules="rows" summary="这是表格摘要">
        <tr>
        <th>Month</th>
        <th>Savings</th>
        </tr>
        <tr>
        <td>January</td>
        <td>$100</td>
        </tr>
    </table>
**Tips : **
    summary 属性不会对普通浏览器中产生任何视觉变化。  
    屏幕阅读器可以利用该属性。  
    由于不会在普通浏览器中产生任何视觉效果，很难判断浏览器是否支持 summary 属性。
    
#### [HTML &lt;caption&gt; 标签](id:caption)


**定义和用法 : **  
`caption` 元素定义表格标题。  
`caption` 标签必须紧随 table 标签之后。您只能对每个表格定义一个标题。通常这个标题会被居中于表格之上。

**例子 : **
    
    <table border="1">
      <caption>我是表格标题</caption>
      <tr>
        <th>Month</th>
        <th>Savings</th>
      </tr>
      <tr>
        <td>January</td>
        <td>$100</td>
      </tr>
    </table>
<table border="1">
  <caption>我是表格标题</caption>
  <tr>
    <th>Month</th>
    <th>Savings</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
  </tr>
</table>


#### [HTML &lt;th&gt; 标签](id:th)


**定义和用法 : **  

定义表格内的表头单元格。

HTML 表单中有两种类型的单元格：

- 表头单元格 - 包含表头信息（由 th 元素创建）
- 标准单元格 - 包含数据（由 td 元素创建）

`th` 元素内部的文本通常会呈现为居中的粗体文本，而 `td` 元素内的文本通常是左对齐的普通文本。


**元素属性说明 : **

属性 | 值 | 描述
----|---|---
abbr | text | 规定单元格中内容的缩写版本。
align | 请看[align](#align)属性介绍 | 规定单元格内容的水平对齐方式。
[axis](#axis) | category_name | 对单元格进行分类。
bgcolor | rgb(x,x,x)  #xxxxxx  colorname | 不推荐使用。请使用样式替代它。  规定表格单元格的背景颜色。
[char](#char) | character | 规定根据哪个字符来进行内容的对齐。
[charoff](#charoff) | number | 规定对齐字符的偏移量。
[colspan](#colspan) | number | 设置单元格可横跨的列数。(***)
headers | idrefs | 由空格分隔的表头单元格 ID 列表， 为数据单元格提供表头信息。
height | pixels % | 不推荐使用。请使用样式替代它。  规定表格单元格的高度。
nowrap | nowrap | 不推荐使用。请使用样式取而代之。  规定单元格中的内容是否折行。
[rowspan](#rowspan) | number | 规定单元格可横跨的行数。(***)
scope | 看[scope](#scope)属性性介绍 | 定义将表头数据与单元数据相关联的方法。
valign | 看[valign](#valign)属性性介绍 | 规定单元格内容的垂直排列方式。
width | pixels % | 不推荐使用。请使用样式取而代之。  规定表格单元格的宽度。  

上面的一些属性会带到 [td](#td) 去讲

[char](id:char) 和 [charoff](id:charoff) : 所有浏览器都不支持，这里就不说了。
[axis](id:axis) : 几乎没有浏览器支持 axis 属性。

**[align](id:align) 属性介绍 : **  
`align` 属性规定表格行中内容的水平对齐方式。  
IE 无法正确地处理 "justify" 值，IE 会以居中的方式进行处理。  
几乎没有浏览器能够正确地处理 "char" 值。  

值 | 描述
----|---
left | 左对齐内容（默认值）。
right | 右对齐内容。
center | 居中对齐内容（th 元素的默认值）。
justify | 对行进行伸展，这样每行都可以有相等的长度（就像在报纸和杂志中）。
char | 将内容对准指定字符。  

**例子 : **

    <table border="1" style="height:200px">
      <tr>
        <th align="left">Company</th>
        <th>Address</th>
        <th>other</th>
      </tr>
      <tr>
        <td align="center">Apple, Inc.</td>
        <td>CA 95014</td>
        <td>其他</td>
      </tr>
    </table>
<table border="1" style="width:400px">
    <tr>
        <th align="left">Company</th>
        <th>Address</th>
        <th>other</th>
    </tr>
    <tr>
        <td align="center">Apple, Inc.</td>
        <td>CA 95014</td>
        <td>其他</td>
    </tr>
</table>

**[scope](id:scope) 属性介绍 : **  
`scope` 属性定义将表头单元与数据单元相关联的方法。  
`scope` 属性标识某个单元是否是列、行、列组或行组的表头。  
`scope` 属性不会在普通浏览器中产生任何视觉变化。  

值 | 描述
----|---
col | 规定单元格是列的表头。
row | 规定单元格是行的表头。
colgroup | 规定单元格是列组的表头。
rowgroup | 规定单元格是行组的表头。

**[valign](id:valign) 属性介绍 : **  

值 | 描述
----|---
top | 对内容进行上对齐。
middle | 对内容进行居中对齐（默认值）。
bottom | 对内容进行下对齐。
baseline | 与基线对齐。

**例子 : **

    <table border="1" style="height:200px">
      <tr>
        <th valign="baseline">Company</th>
        <th>Address</th>
      </tr>
      <tr>
        <td valign="bottom">Apple, Inc.</td>
        <td>1 Infinite Loop Cupertino, CA 95014</td>
      </tr>
    </table>
<table border="1" style="height:200px">
    <tr>
        <th valign="baseline">Company</th>
        <th>Address</th>
    </tr>
    <tr>
        <td valign="bottom">Apple, Inc.</td>
        <td>1 Infinite Loop Cupertino, CA 95014</td>
    </tr>
</table>

#### [HTML &lt;tr&gt; 标签](id:tr)


**定义和用法 : **  

`<tr>` 标签定义 HTML 表格中的行。  
tr 元素包含一个或多个 th 或 td 元素。

**元素属性说明 : **

属性 | 值 | 描述
----|---|---
align | 查看[align](#align)属性介绍 | 定义表格行的内容对齐方式。
bgcolor | rgb(x,x,x) #xxxxxx colorname | **不赞成使用**。请使用样式取而代之。 规定表格行的背景颜色。
[char](#char) | character | 规定根据哪个字符来进行文本对齐。  几乎没有浏览器支持 char 属性。
[charoff](#charoff) | number | 规定第一个对齐字符的偏移量。  几乎没有浏览器支持 charoff 属性。
valign | 查看[valign](#valign)属性介绍 | 规定表格行中内容的垂直对齐方式。


#### [HTML &lt;td&gt; 标签](id:td)


**定义和用法 : **  

`<td>` 标签定义 HTML 表格中的标准单元格。

HTML 表格有两类单元格：  

- 表头单元 - 包含头部信息（由 th 元素创建）
- 标准单元 - 包含数据（由 td 元素创建）

td 元素中的文本一般显示为正常字体且左对齐。

**元素属性说明 : **

属性 | 值 | 描述
----|---|---
abbr | text | 规定单元格中内容的缩写版本。
align | 查看[align](#align)属性介绍 | 规定单元格内容的水平对齐方式。
[axis](#axis) | category_name | 对单元进行分类。
bgcolor | rgb(x,x,x) #xxxxxx colorname | **不赞成使用**  请使用样式取而代之。  规定单元格的背景颜色。
[char](#char) | character | 规定根据哪个字符来进行内容的对齐。
[charoff](#charoff) | number | 规定对齐字符的偏移量。
colspan | number | 规定单元格可横跨的列数。
headers | `header_cells'_id` | 规定与单元格相关的表头。
height | pixels % | **不赞成使用**。请使用样式取而代之。  规定表格单元格的高度。
nowrap | nowrap | **不赞成使用**。请使用样式取而代之。  规定单元格中的内容是否折行。
rowspan | number | 规定单元格可横跨的行数。
scope | 查看[scope](#scope)属性介绍 | 定义将表头数据与单元数据相关联的方法。
valign | 查看[valign](#scope)属性介绍 | 规定单元格内容的垂直排列方式。
width | pixels % | **不赞成使用**。  请使用样式取而代之。   规定表格单元格的宽度。


**例子 : **

        
    <table border="2px">
        <tr>
            <th>班级</th>
            <th>姓名</th>
            <th>分数</th>
            <th>电话</th>
            <th>手机号</th>
            <th>地址</th>
        </tr>
        <tr>
            <td rowspan="2">二年级</td>
            <td>张力</td>
            <td>85</td>
            <td colspan="2">1523432341</td>
            <td></td>
        </tr>
        <tr>
            <td>张晓</td>
            <td>95</td>
            <td colspan="2">1533233351</td>
            <td> &nbsp; </td>
        </tr>
    </table>


<table border="2px">
    <tr>
        <th>班级</th>
        <th>姓名</th>
        <th>分数</th>
        <th>电话</th>
        <th>手机号</th>
        <th>地址</th>
    </tr>
    <tr>
        <td rowspan="2">二年级</td>
        <td>张力</td>
        <td>85</td>
        <td colspan="2">1523432341</td>
        <td></td>
    </tr>
    <tr>
        <td>张晓</td>
        <td>95</td>
        <td colspan="2">1533233351</td>
        <td> &nbsp; </td>
    </tr>
</table>

    <table cellpadding="10px" border="1px" cellspacing="0" width="400px" height="200px">
        <tr>
            <td>介绍</td>
            <td valign="top">
                <table cellpadding="0" border="1px" cellspacing="10px" width="200px" height="100px">
                    <tr>
                        <td>说明 : </td>
                        <td>保修 : </td>
                    </tr>
                    <tr>
                        <td>xxx</td>
                        <td>一个月</td>
                    </tr>
                </table>
            </td>
        </tr>
</table>
<table cellpadding="10px" border="1px" cellspacing="0" width="200px" height="100px">
    <tr>
        <td>介绍</td>
        <td valign="top">
            <table cellpadding="0" border="1px" cellspacing="10px" width="100px" height="50px">
                <tr>
                    <td>说明 : </td>
                    <td>保修 : </td>
                </tr>
                <tr>
                    <td>xxx</td>
                    <td>一个月</td>
                </tr>
            </table>
        </td>
    </tr>
</table>    

**`Tips : `**使用 "&nbsp;" 处理没有内容的单元格。


#### [HTML &lt;thead&gt; 标签](id:thead)


**定义和用法 : **  

`<thead>` 标签定义表格的表头。该标签用于组合 HTML 表格的表头内容。  
`thead` 元素应该与 `tbody` 和 `tfoot` 元素结合起来使用。  
`tbody` 元素用于对 HTML 表格中的主体内容进行分组，而 `tfoot` 元素用于对 HTML 表格中的表注（页脚）内容进行分组。  

**元素属性说明 : **

属性 | 值 | 描述
----|---|---
align |	查看[align](#align)属性说明  | 定义 thead 元素中内容的对齐方式。
[char](#char) | character | 规定根据哪个字符来进行文本对齐。
[charoff](#charoff) | number | 规定第一个对齐字符的偏移量。
valign | 查看[valign](#valign)属性说明 | 规定 thead 元素中内容的垂直对齐方式。

<table border="1">
  <thead>
    <tr>
      <th>月份</th>
      <th>收入</th>
    </tr>
  </thead>

  <tfoot>
    <tr>
      <td>总计 : </td>
      <td>$180</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>一月</td>
      <td>$100</td>
    </tr>
    <tr>
      <td>二月</td>
      <td>$80</td>
    </tr>
  </tbody>
</table>







#### [HTML &lt;tbody&gt; 标签](id:tbody)


**定义和用法 : **  
`<tbody>` 标签表格主体（正文）。该标签用于组合 HTML 表格的主体内容。


**元素属性说明 : **

属性 | 值 | 描述
----|---|---
属性与 [thead](#thead) 同 |---|---


#### [HTML &lt;tfoot&gt; 标签](id:tfoot)


**定义和用法 : **  
`<tfoot>` 标签定义表格的页脚（脚注或表注）。该标签用于组合 HTML 表格中的表注内容。


**元素属性说明 : **

属性 | 值 | 描述
----|---|---
属性与 [thead](#thead) 同 |---|---
 




#### [HTML &lt;col&gt; 标签](id:col)


**定义和用法 : **  
`<col>` 标签为表格中一个或多个列定义属性值。  
如需对全部列应用样式，<col> 标签很有用，这样就不需要对各个单元和各行重复应用样式了。  
您只能在 table 或 colgroup 元素中使用 <col> 标签。


**元素属性说明 : **

属性 | 值 | 描述
----|---|---
align | 查看[align](#align)属性介绍 | 规定与 col 元素相关的内容的水平对齐方式。
[char](#char) | character | 规定根据哪个字符来对齐与 col 元素相关的内容。
[charoff](#charoff) | number | 规定第一个对齐字符的偏移量。
span | number | 规定 col 元素应该横跨的列数。
valign | 查看[valign](#vlign)属性介绍 | 定义与 col 元素相关的内容的垂直对齐方式。
width | pixels,  %,  relative_length | 规定 col 元素的宽度。


**例子 : **

    <table width="100%" border="1">
      <col span="3" align="right" />
      <tr>
        <th>ISBN</th>
        <th>Title</th>
        <th>Price</th>
      </tr>
      <tr>
        <td>3476896</td>
        <td>My first HTML</td>
        <td>$53</td>
      </tr>
    </table>
<table width="100%" border="1">
  <col span="3" align="right" />
  <tr>
    <th>ISBN</th>
    <th>Title</th>
    <th>Price</th>
  </tr>
  <tr>
    <td>3476896</td>
    <td>My first HTML</td>
    <td>$53</td>
  </tr>
</table>


`<col>` 标签，如同 `<colgroup>` 标签中的 span 属性一样，允许设置设置 `<col>` 标签能够影响多少列。  
在默认情况下，它只能影响一列。举个例子，创建一个有 5 列的 `<colgroup>`。我们分别将第一列和最后一列靠左和靠右对齐，中间的三列居中。  

        <colgroup>
          <col align="left" />
          <col align="center" span="3" />
          <col align="right" />
        </colgroup>


#### [HTML &lt;colgroup&gt; 标签](id:colgroup)


**定义和用法 : **  
`<colgroup>` 标签用于对表格中的列进行组合，以便对其进行格式化。  
如需对全部列应用样式，<colgroup> 标签很有用，这样就不需要对各个单元和各行重复应用样式了。  
`<colgroup>` 标签只能在 table 元素中使用。  
请为 <colgroup> 标签添加 class 属性。这样就可以使用 CSS 来负责对齐方式、宽度和颜色等等。  

**元素属性说明 : **

属性 | 值 | 描述
----|---|---
align | 查看[align](#align)属性介绍 | 规定与 col 元素相关的内容的水平对齐方式。
[char](#char) | character | 规定根据哪个字符来对齐与 col 元素相关的内容。
[charoff](#charoff) | number | 规定第一个对齐字符的偏移量。
span | number | 规定 col 元素应该横跨的列数。
valign | 查看[valign](#vlign)属性介绍 | 定义与 col 元素相关的内容的垂直对齐方式。
width | pixels,  %,  relative_length | 规定 col 元素的宽度。

**例子 : **
两个 colgroup 元素为表格中的三列规定了不同的对齐方式和样式（注意第一个 colgroup 元素横跨两列）：

    <table width="100%" border="1">
      <colgroup span="2" align="left"></colgroup>
      <colgroup align="right" style="color:#0000FF;"></colgroup>
      <tr>
        <th>ISBN</th>
        <th>Title</th>
        <th>Price</th>
      </tr>
      <tr>
        <td>3476896</td>
        <td>My first HTML</td>
        <td>$53</td>
      </tr>
    </table>

<table width="100%" border="1">
  <colgroup span="2" align="left"></colgroup>
  <colgroup align="right" style="color:#0000FF;"></colgroup>
  <tr>
    <th>ISBN</th>
    <th>Title</th>
    <th>Price</th>
  </tr>
  <tr>
    <td>3476896</td>
    <td>My first HTML</td>
    <td>$53</td>
  </tr>
</table>
















