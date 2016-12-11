### HTML_Form表单

**表单标签**

标签 | 描述
----|---
[`<form>`](#form) | 定义供用户输入的表单(***)
[`<input>`](#input) | 定义输入域(***)
[`<textarea>`](#textarea) | 定义文本域(一个多行的输入控件)(***)
[`<label>`](#label) | 定义一个控制的标签(**)
[`<fieldset>`](#fieldset) | 定义域(*)
[`<legend>`](#legend) | 定义域的标题(*)
[`<select>`](#select) | 定义一个选择列表(***)
[`<optgroup>`](#optgroup) | 定义选项组(*)
[`<option>`](#option) | 定义下拉列表中的选项(***)
[`<button>`](#button) | 定义一个按钮(**)


#### [HTML &lt;form&gt; 标签](id:form)
**定义和用法**
<form> 标签用于为用户输入创建 HTML 表单。  
表单能够包含 input 元素，比如文本字段、复选框、单选框、提交按钮等等。  
表单还可以包含 menus、textarea、fieldset、legend 和 label 元素。  
表单用于向服务器传输数据。    
**`Tips : `**form 元素是块级元素，其前后会产生折行。

**元素属性**

属性 | 值 | 描述
----|---|---
`accept` | MIME_type | HTML 5 中不支持。
`accept-charset` | charset_list | 规定服务器可处理的表单数据字符集。`[UTF-8,gb2312]`(*)
`action` | URL | 规定当提交表单时向何处发送表单数据。(***)
`autocomplete` | on/off | 规定是否启用表单的自动完成功能。(*)
`enctype` | 见说明 | 规定在发送表单数据之前如何对其进行编码。(***)
`method` | get/post | 规定用于发送 form-data 的 HTTP 方法。(***)
`name` | form_name | 规定表单的名称，这个基本每个标签都有的属性。(**)
`novalidate` | novalidate | 	如果使用该属性，则提交表单时不进行验证。(**)
`target` | 见说明 | 规定在何处打开 action URL。(**)


[**`target`**](id:target) 属性分析 :   

编码方式 | 描述
----|--- 
`_blank` | 表单提交后跳转新页面 (**)
`_self` | (默认)表单提交后当前页面跳转(**)
`_parent` | 提交表单后父框架集跳转
`_top` | 提交表单后顶级页面(整个窗口)跳转
`framename` |	在指定的框架中打开。 (**)


[**`enctype`**](id:enctype) 属性可能的值：  

编码方式 | 描述 
----|---
`application/x-www-form-urlencoded` | 在发送前编码所有字符（默认）
`multipart/form-data` | 不对字符编码。但如果表单有***上传文件***操作，必须使用该值。
`text/plain` | 空格转换为 "+" 加号，但不对特殊字符编码。

**例子 : **

        <form action="form_action.asp" method="get" 
            enctype="application/x-www-form-urlencoded" target="_blank">
          First name: <input type="text" name="fname" />
          Last name: <input type="text" name="lname" />
          <input type="submit" value="Submit" />
        </form>

#### [HTML &lt;input&gt; 标签](id:input)
**定义和用法**
&lt;input&gt; 标签用于搜集用户信息。

根据不同的 type 属性值，输入字段拥有很多种形式。输入字段可以是文本字段、复选框、掩码后的文本控件、单选按钮、按钮等等。

**元素属性**

属性 | 值 | 描述 
----|---|---
accept | mime_type | 规定通过文件上传来提交的文件的类型(**)。
align | left ,right ,top ,middle ,bottom | 不赞成使用,可以用样式来调节。规定图像输入的对齐方式。
alt | text | 定义图像输入的替代文本。
autocomplete | on, off | 规定是否使用输入字段的自动完成功能。
autofocus | autofocus | 规定输入字段在页面加载时是否获得焦点。 （不适用于 type="hidden"）
checked | checked | 规定此 input 元素首次加载时应当被选中。(**)
disabled | disabled | 当 input 元素加载时禁用此元素。(**)
form | formname | 规定输入字段所属的一个或多个表单。
formaction | URL | 覆盖表单的 action 属性。 （适用于 type="submit" 和 type="image"）
formenctype | 同form的[target](#enctype)属性 | 覆盖表单的 enctype 属性。 （适用于 type="submit" 和 type="image"）
formmethod | get, post | 覆盖表单的 method 属性。 （适用于 type="submit" 和 type="image"）
formnovalidate | formnovalidate | 覆盖表单的 novalidate 属性。 如果使用该属性，则提交表单时不进行验证。
formtarget | 同form的[target](#target)属性 | 覆盖表单的 target 属性。（适用于 type="submit" 和 type="image"）
height | pixels % | 定义 input 字段的高度。（适用于 type="image"）
list | datalist-id | 引用包含输入字段的预定义选项的 datalist 。
max | number date | 规定输入字段的最大值。 请与 "min" 属性配合使用，来创建合法值的范围。
maxlength | number | 规定输入字段中的字符的最大长度。(**)
min | number date | 规定输入字段的最小值。 请与 "max" 属性配合使用，来创建合法值的范围。
multiple | multiple | 如果使用该属性，则允许一个以上的值。(**)
name | field_name | 定义 input 元素的名称。(**)
pattern | regexp_pattern | 规定输入字段的值的模式或格式。 例如 pattern="[0-9]" 表示输入值必须是 0 与 9 之间的数字。
placeholder | text | 规定帮助用户填写输入字段的提示。
readonly | readonly | 规定输入字段为只读。(**)
required | required | 指示输入字段的值是必需的。
size | number_of_char | 定义输入字段的宽度。(**)
src | URL | 定义以提交按钮形式显示的图像的 URL。
step | number | 规定输入字的的合法数字间隔。
type | 看type说明表 | 规定 input 元素的类型。(***)
value | value | 规定 input 元素的值。(**)
width | pixels % | 定义 input 字段的宽度。（适用于 type="image"）


**type说明表 **    

值 | 描述
----|---
[button](#button) | 定义可点击按钮（多数情况下，用于通过 JavaScript 启动脚本）。(*)
[checkbox](#checkbox) | 定义复选框。(***)
[file](#file) | 定义输入字段和 "浏览"按钮，供文件上传。(***)
[hidden](#hidden) | 定义隐藏的输入字段。(*)
[image](#image) | 定义图像形式的提交按钮。(*)
[password](#password) | 定义密码字段。该字段中的字符被掩码。(*)
[radio](#radio) | 定义单选按钮。(***)
[reset](#reset) | 定义重置按钮。重置按钮会清除表单中的所有数据。(*)
[submit](#submit) | 定义提交按钮。提交按钮会把表单数据发送到服务器。(***)
[text](#text) | 定义单行的输入字段，用户可在其中输入文本。默认宽度为 20 个字符。(***)
 
[**Text**](id:text)  
`<input type="text" />` 定义用户可输入文本的单行输入字段。  

**例子 : **
        
        Email: <input type="text" name="email" />
Email: <input type="text" name="email" />

        Email: <input type="text" name="email" disabled="disabled" value="置为失效，你就只能看不能改了"/>

Email: <input type="text" name="email" disabled="disabled" value="置为失效，你就只能看不能改了"/>

**`Tips : `** 其他标签也可以置为失效,只能查看，不能操作、修改。如 radio/checkbox/textarea/submit等。

[**Button**](id:button)  
`<input type="button" />` 定义可点击的按钮，但没有任何行为。button 类型常用于在用户点击按钮时启动 JavaScript 程序。  
**例子 : **  
        
        <script>
            function msg()
            {
                alert('显示消息');
            }
        </sctipt>
        <input type="button" value="Click me" onclick="msg()" />
<input type="button" value="点击我试试" onclick="msg()" />

[**Checkbox**](id:checkbox)  
`<input type="checkbox" />` 定义复选框。复选框允许用户在一定数目的选择中选取一个或多个选项。  

        
**例子 : **  

        <input type="checkbox" name="vehicle" value="Bike" /> I have a bike
        <input type="checkbox" name="vehicle" value="Car" /> I have a car

<input type="checkbox" name="vehicle" value="Bike" /> I have a bike  
<input type="checkbox" name="vehicle" value="Car" /> I have a car  

**`Tips : `**这里的name属性必须一致才能归到一组，因为一个表单中可以有好几组复选框表单。

[**File**](id:file)  
`<input type="file" />` 用于文件上传。  

**例子 : **  

        <input type="file" name="pic" accept="image/gif" />
<input type="file" name="pic" accept="image/gif" />

**`Tips : `** form 的 `enctype` 属性必须为 `multipart/form-data` 该表单才能成功上传文件。


[**Hidden**](id:hidden)  
`<input type="hidden" />` 定义隐藏字段。隐藏字段对于用户是不可见的。隐藏字段通常会存储一个默认值，它们的值也可以由 JavaScript 进行修改。  
 
 **例子 : **  
 
        <input type="hidden" name="country" value="Norway" />,因为是隐藏的，所以这里什么都看不到
<input type="hidden" name="country" value="Norway" />,因为是隐藏的，所以这里什么都看不到

[**Image**](id:image)
`<input type="image" />` 定义图像形式的提交按钮。  
必须把 `src` 属性 和 `alt` 属性 与 `<input type="image">` 结合使用。  
`alt` 用于无法显示图片时显示的提示语。
 **例子 : **   
        
        <input type="image" src="submit.gif" alt="Submit" />
<input type="image" src="submit.gif" alt="Submit" />

**`Tips : `** image 标签可以把鼠标点击图片位置以 `&x={number}&y={number}` 格式提交到服务器端。


[**Password**](id:password)  
`<input type="password" />` 定义密码字段。密码字段中的字符会被掩码（显示为星号或原点）。

 **例子 : **  
 
     <input type="password" name="pwd" value="123"/>与text区别是值是被原点代替，用于保密。
<input type="password" name="pwd" value="123"/>与**`text`**区别是值是被原点代替，用于保密。  

[**Radio**](id:radio)  
`<input type="radio" />` 定义单选按钮。单选按钮允许用户选取给定数目的选择中的一个选项。

 **例子 : **  
 
        <input type="radio" name="sex" value="man" /> 男性
        <input type="radio" name="sex" value="woman" /> 女性
<input type="radio" name="sex" value="man" checked="checked"/> 男性(默认被选中)  
<input type="radio" name="sex" value="woman" /> 女性

**`Tips : `**这个同 `Checkbox` 相似，是将 **name** 设置为相同的值时，为一组。 `Radio` 类型一组只能选中一个选项，此点与 `Checkbox` 不同。

[**Reset Button**](id:reset)  

`<input type="reset" />` 定义重置按钮。重置按钮会清除表单中的所有数据。

 **例子 : **    

        <form action="form_action.asp" method="post">
        Email: <input type="text" name="email" />
        <input type="reset" value="重新填写"/>
        </form>

<form action="form_action.asp" method="post">
Email: <input type="text" name="email" />
<input type="reset" value="重新填写"/>
</form>

[**Submit**](id:submit)  

`<input type="submit" />` 定义提交按钮。提交按钮用于向服务器发送表单数据。数据会发送到表单的 action 属性中指定的页面。  

 **例子 : **    

        <form action="https://www.baidu.com" method="get">
        Email: <input type="text" name="email" />
        <input type="submit" value="提交"/>
        </form>
<form action="https://www.baidu.com" method="get">
Email: <input type="text" name="email" />
<input type="reset" value="重新填写"/>
<input type="submit" value="提交"/>
</form>
**`Tips : `** 将form标签中的表单标签数据提交到服务器，包括 input, textarea, select等。

#### [HTML &lt;textarea&gt; 标签](id:textarea)
**定义和用法**  
&lt;textarea&gt;  标签定义多行的文本(长文本)输入控件。

文本区中可容纳无限数量的文本，其中的文本的默认字体是等宽字体（通常是 Courier）。

可以通过 cols 和 rows 属性来规定 textarea 的尺寸，不过更好的办法是使用 CSS 的 height 和 width 属性。

**Tips : ** 在文本输入区内的文本行间，用 "%OD%OA" （回车/换行）进行分隔。
**元素属性**

属性 | 值 | 描述 
----|---|---
autofocus | autofocus | 规定在页面加载后文本区域自动获得焦点。
cols | number | 规定文本区内的可见宽度。(**)
disabled | disabled | 规定禁用该文本区。(*)
form | form_id | 规定文本区域所属的一个或多个表单。(*)
maxlength | number | 规定文本区域的最大字符数。(**)
name | name_of_textarea | 规定文本区的名称。(*)
placeholder | text | 规定描述文本区域预期值的简短提示。
readonly | readonly | 规定文本区为只读。
required | required | 规定文本区域是必填的。(*)
rows | number | 规定文本区内的可见行数。(**)
wrap | hard, soft | 规定当在表单中提交时，文本区域中的文本如何换行。(*)

**wrap 属性**  

值 | 描述
---|---
soft | 当在表单中提交时，textarea 中的文本不换行。默认值。
hard | 当在表单中提交时，textarea 中的文本换行（包含换行符）。当使用 "hard" 时，必须规定 cols 属性(列数)。

**例子 : **

        <textarea placeholder="请介绍自己...">
        </textarea> 
<textarea placeholder="请介绍自己...">
</textarea> 

        <textarea placeholder="请介绍自己...">
            填写的内容在这里
        </textarea> 
<textarea placeholder="请介绍自己...">
    填写的内容在这里
</textarea> 


#### [HTML &lt;label&gt; 标签](id:label)
**定义和用法**  
&lt;label&gt;  标签为 input 元素定义标注（标记）。

label 元素不会向用户呈现任何特殊效果。不过，它为鼠标用户改进了可用性。如果您在 label 元素内点击文本，就会触发此控件。就是说，当用户选择该标签时，浏览器就会自动将焦点转到和标签相关的表单控件上。

`<label>` 标签的 for 属性应当与相关元素的 id 属性相同。  

**元素属性**

属性 | 值 | 描述 
----|---|---
for | id | 规定 label 绑定到哪个表单元素。
form | formid | 规定 label 字段所属的一个或多个表单。
  
**例子 : ** 这里只点击label标签就可以，而不用再仔细找好再点击input:radio标签了。
        
        <label for="male">man</label>
        <input type="radio" name="sex" id="man" />
        <label for="female">woman</label>
        <input type="radio" name="sex" id="woman" />
        
<label for="male">man</label>
<input type="radio" name="sex" id="man" />
<label for="female">woman</label>
<input type="radio" name="sex" id="woman" />

#### [HTML &lt;fieldset&gt; 标签](id:fieldset)
**定义和用法**  
&lt;fieldset&gt; 元素可将表单内的相关元素分组。

`<fieldset>` 标签将表单内容的一部分打包，生成一组相关表单的字段。

当一组表单元素放到 `<fieldset>` 标签内时，浏览器会以特殊方式来显示它们，它们可能有特殊的边界、3D 效果，或者甚至可创建一个子表单来处理这些元素。

`<fieldset>` 标签没有必需的或唯一的属性。

`<legend>` 标签为 fieldset 元素定义标题。

**元素属性**

属性 | 值 | 描述 
----|---|---
disabled | disabled | 规定应该禁用 fieldset。
form | form_id | 规定 fieldset 所属的一个或多个表单。
name | value | 规定 fieldset 的名称。  

#### [HTML &lt;legend&gt; 标签](id:legend)
**定义和用法**  
&lt;legend&gt; legend 元素为 fieldset 元素定义标题（caption）。

**例子 : **

        <fieldset>
        <legend>域标题</legend>
        域内容
        </filedset>

<fieldset>
<legend>域标题</legend>
域内容
</fieldset>

#### [HTML &lt;select&gt; 标签](id:select)
**定义和用法**  
&lt;select&gt; 元素可创建单选或多选菜单。

`<select&>` 元素中的 `<option>` 标签用于定义列表中的可用选项。

**元素属性**

属性 | 值 | 描述 
----|---|---
autofocus | autofocus | 规定在页面加载后文本区域自动获得焦点。
disabled | disabled | 规定禁用该下拉列表。(*)
form | form_id | 规定文本区域所属的一个或多个表单。(*)
multiple | multiple | 规定可选择多个选项。(**)
name | name | 规定下拉列表的名称。(*)
required | required | 规定文本区域是必填的。(*)
size | number | 规定下拉列表中可见选项的数目。(**)

**例子 : ** 请看 option 标签。



#### [HTML &lt;optgroup&gt; 标签](id:optgroup)
**定义和用法**  
&lt;optgroup&gt; 标签定义选项组。

optgroup 元素用于组合选项。当您使用一个长的选项列表时，对相关的选项进行组合会使处理更加容易。 

**元素属性**

属性 | 值 | 描述 
----|---|---
label | text | 为选项组规定描述。
disabled | disabled | 规定禁用该选项组。

**例子 : **

        <select>
          <optgroup label="Swedish Cars">
            <option value ="volvo">Volvo</option>
            <option value ="saab">Saab</option>
          </optgroup>
        
          <optgroup label="German Cars">
            <option value ="mercedes">Mercedes</option>
            <option value ="audi">Audi</option>
          </optgroup>
        </select>

<select>
  <optgroup label="Swedish Cars">
    <option value ="volvo">Volvo</option>
    <option value ="saab">Saab</option>
  </optgroup>
 
  <optgroup label="German Cars">
    <option label="标记" value ="mercedes">Mercedes</option>
    <option value ="audi">Audi</option>
  </optgroup> 
</select>



#### [HTML &lt;option&gt; 标签](id:option)
**定义和用法**  
&lt;option&gt; 元素定义下拉列表中的一个选项（一个条目）。

浏览器将 `<option>` 标签中的内容作为 `<select>` 标签的菜单或是滚动列表中的一个元素显示。

option 元素位于 select 元素内部。  
**元素属性**

属性 | 值 | 描述 
----|---|---
disabled | disabled | 规定此选项应在首次加载时被禁用。
label | text | 定义当使用 `<optgroup>` 时所使用的标注。
selected | selected | 规定选项（在首次显示在列表中时）表现为选中状态。(**)
value | text | 定义送往服务器的选项值。(**)

**例子 1: ** 这是多选形式的选择列表。

        <select  name="fruit" multiple="multiple" size="4">
            <option value="apple">苹果</option>
            <option value="banana">香蕉</option>
            <option value="watermelon">西瓜</option>
            <option value="pear">梨</option>
            <option value="peach">桃子</option>
        </select>
<select  name="fruit" multiple="multiple" size="4">
    <option value="apple">苹果</option>
    <option value="banana" selected="selected">香蕉</option>
    <option value="watermelon" selected="selected">西瓜</option>
    <option value="pear">梨</option>
    <option value="peach">桃子</option>
</select>  

**例子 2: ** 这是单选形式的选择列表。

        <select name="fruit" >
            <option value="apple">苹果</option>
            <option value="banana">香蕉</option>
            <option value="watermelon">西瓜</option>
            <option value="pear">梨</option>
            <option value="peach">桃子</option>
        </select>
<select name="fruit">
    <option value="apple">苹果</option>
    <option value="banana">香蕉</option>
    <option value="watermelon">西瓜</option>
    <option value="pear">梨</option>
    <option value="peach">桃子</option>
</select>  


#### [HTML &lt;button&gt; 标签](id:button)
**定义和用法**  
&lt;button&gt; 标签定义一个按钮。

在 button 元素内部，您可以放置内容，比如文本或图像。这是该元素与使用 input 元素创建的按钮之间的不同之处。

`<button>` 控件 与 `<input type="button">` 相比，提供了更为强大的功能和更丰富的内容。`<button>` 与 `</button>` 标签之间的所有内容都是按钮的内容，其中包括任何可接受的正文内容，比如文本或多媒体内容。例如，我们可以在按钮中包括一个图像和相关的文本，用它们在按钮中创建一个吸引人的标记图像。

唯一禁止使用的元素是图像映射，因为它对鼠标和键盘敏感的动作会干扰表单按钮的行为。

请始终为按钮规定 type 属性。Internet Explorer 的默认类型是 "button"，而其他浏览器中（包括 W3C 规范）的默认值是 "submit"。  
 
**元素属性**

属性 | 值 | 描述 
----|---|---
autofocus | autofocus | 规定当页面加载时按钮应当自动地获得焦点。
disabled | disabled | 规定应该禁用该按钮。
form | form_name | 规定按钮属于一个或多个表单。
formaction | url | 覆盖 form 元素的 action 属性。 注释：该属性与 type="submit" 配合使用。
formenctype | 见form标签[enctype](#enctype) | 覆盖 form 元素的 enctype 属性。 注释：该属性与 type="submit" 配合使用。
formmethod | get,post | 覆盖 form 元素的 method 属性。 注释：该属性与 type="submit" 配合使用。
formnovalidate | formnovalidate | 覆盖 form 元素的 novalidate 属性。 注释：该属性与  type="submit" 配合使用。
formtarget | 见form标签[target](#target) | 覆盖 form 元素的 target 属性。 注释：该属性与 type="submit" 配合使用name | button_name | 规定按钮的名称。
type | button,reset,submit | 规定按钮的类型。
value | text | 规定按钮的初始值。可由脚本进行修改。




