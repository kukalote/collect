`status (status, st)`: 显示工作副本中目录与文件的状态。

    用法: status [PATH...]

 
    未指定参数时，只显示本地修改的条目(没有网络访问)。
    
    使用 -q 时，只显示本地修改条目的摘要信息。
    
    使用 -u 时，增加工作版本和服务器上版本过期信息。
    
    使用 -v 时，显示每个条目的完整版本信息。

 

输出的前七栏各占一个字符宽度: 

第一栏: 表示一个项目是增加、删除，还是修改

      “ ” 无修改

      “A” 增加

      “C” 冲突

      “D” 删除

      “I” 忽略

      “M” 改变

      “R” 替换

      “X” 未纳入版本控制的目录，被外部引用的目录所创建

      “?” 未纳入版本控制

      “!” 该项目已遗失(被非 svn 命令删除)或不完整

      “~” 版本控制下的项目与其它类型的项目重名

第二栏: 显示目录或文件的属性状态

      “ ” 无修改

      “C” 冲突

      “M” 改变

第三栏: 工作副本目录是否被锁定

      “ ” 未锁定

      “L” 锁定

第四栏: 已调度的提交是否包含副本历史

      “ ” 没有历史

      “+” 包含历史

第五栏: 该条目相对其父目录是否已切换，或者是外部引用的文件

      “ ” 正常

      “S” 已切换

      “X” 被外部引用创建的文件

第六栏: 版本库锁定标记

      (没有 -u)

      “ ” 没有锁定标记

      “K” 存在锁定标记

      (使用 -u)

      “ ” 没有在版本库中锁定，没有锁定标记

      “K” 在版本库中被锁定，存在锁定标记

      “O” 在版本库中被锁定，锁定标记在一些其他工作副本中

      “T” 在版本库中被锁定，存在锁定标记但已被窃取

      “B” 没有在版本库中被锁定，存在锁定标记但已被破坏

第七栏: 项目冲突标记

      “ ” 正常

      “C” 树冲突

    如果项目包含于树冲突之中，在项目状态行后会附加行，说明冲突的种类。

 

是否过期的信息出现的位置是第九栏(与 -u 并用时): 

      “*” 服务器上有更新版本

      “ ” 工作副本是最新版的

 

剩余的栏位皆为变动宽度，并以空白隔开: 

    工作版本号(使用 -u 或 -v 时)

    最后提交的版本与最后提交的作者(使用 -v 时)

    工作副本路径总是最后一栏，所以它可以包含空白字符。