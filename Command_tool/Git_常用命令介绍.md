# Git 常用命令介绍

### 常用命令

1. 查看远程git地址

   ```bash
   git remote -v
   ```

2. 查看远程git版本库

   ```bash
   git remote show origin 
   ```

3. 提交前恢复文件内容

   ```bash
   git co file
   ```

4. 恢复未push的提交

   ```bash
   git reset file
   ```

   



#### 配置仓库

##### **`git config`** 可获得和设置仓库或全局选项

这里就要讲一下git的配置文件了: 

配置文件地址 | 命令选项 | 作用
---|---|---
`/etc/gitconfig` | `--system` | 是系统上用户git仓库的通用配置文件
`~/.gitconfig` 或 `~/.config/git/config` | `--global` | 只针对当前用户
`.git/config`(Git 仓库目录中的config文件) | `--local` | 针对该仓库

常用命令如下    
    //配置全局选项
    $ git config --global user.name "shuimeng"
    $ git config --global user.email shuimeng@qq.com
    
    //查看指定配置
    $ git config user.name
    
    //查看当前配置列表
    $ git config --list
    
    //还可以删除指定的选项
    $ git config --global --unset user.name
    //或全部删除
    $ git config --global --unset-all


#### 获取或者创建项目

##### **`git init`** 将一个目录初始化为 Git 仓库

    $ git init //可以将一个目录初始化或重新初始化

##### **`git clone [url]`** 克隆现有的仓库

    $ git clone https://github.com/libgit2/libgit2 mylibgit

这会在当前目录下创建一个名为 “mylibgit” 的目录，并在这个目录下初始化一个 .git 文件夹，从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。 如果你进入到这个新建的 mylibgit 文件夹，你会发现所有的项目文件已经在里面了

#### 基础快照

##### **`git add`** 将文件加入跟踪, 保证下次 commit 将文件移至暂存区。  

`git add` 默认会跳过被忽略的文件, 但也可以用 `-f` 选项指定提交忽略文件。

##### **`git status`** 查看工作目录的状态

`git status -s` 可以以简短格式 `[--short]` 格式显示

X | Y | Meaning
------|--------|----------------------------------
      |  `[MD]`| not updated
M     |  `[MD]`| updated in index
A     |  `[MD]`| added to index
D     |  `[M]` | deleted from index
R     |  `[MD]`| renamed in index
C     |  `[MD]`| copied in index
`[MARC]` |     | index and work tree matches
`[MARC]` |  M  | work tree changed since index
`[MARC]` |  D  | deleted in work tree
---------|-----|----------------------------------
D        |  D  | unmerged, both deleted
A        |  U  | unmerged, added by us
U        |  D  | unmerged, deleted by them
U        |  A  | unmerged, added by them
D        |  U  | unmerged, deleted by us
A        |  A  | unmerged, both added
U        |  U  | unmerged, both modified
---------|-----|--------------------------------
?        |  ?  | untracked
!        |  !  | ignored

**`git commit`** 将修改记录到暂存区

将当前被跟踪的内容保存到暂存区, 同时添加一条新commit并写入修改日志。

操作情况一般有 : 

1. 使用 `git add` 添加文件跟踪, 后再提交
2. 使用 `git rm` 取消文件跟踪后, 再提交
3. 将已存在 git 库中的文件修改后提交

常用选项 : 

    $ git commit -m 'msgs'

> 尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。 Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 git commit 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤

    $ git commit -a -m 'added new benchmarks'

##### **`git diff`** 显示提交版本之间的区别, 提交和副本之间的区别

要查看尚未暂存的文件更新了哪些部分，不加参数直接输入 `git diff`:

    $ git diff
> 此命令比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。

若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --cached` 命令。（Git 1.6.1 及更高版本还允许使用 `git diff --staged`，效果是相同的，但更好记些。）

    $ git diff --staged
> 请注意，`git diff` 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。 所以有时候你一下子暂存了所有更新过的文件后，运行 `git diff` 后却什么也没有，就是这个原因。

##### **`git rm`** 从暂存区移除文件

1. 要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

    如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 “Changes not staged for commit” 部分（也就是 未暂存清单）, 然后再运行 `git rm` 记录此次移除文件的操作。

    > 下一次提交时，该文件就不再纳入版本管理了。 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。
    
2. 我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 `.gitignore` 文件，不小心把一个很大的日志文件或一堆 .a 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 `--cached` 选项：

        $ git rm --cached README

    `git rm` 命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。 比方说：

        $ git rm log/\*.log

    > 注意到星号 * 之前的反斜杠 \， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。 此命令删除 log/ 目录下扩展名为 .log 的所有文件。 类似的比如：

        $ git rm \*~

    > 该命令为删除以 ~ 结尾的所有文件。

##### **`git mv`** 移动文件


当你看到 Git 的 mv 命令时一定会困惑不已。 要在 Git 中对文件改名，可以这么做：

    $ git mv file_from file_to

它会恰如预期般正常工作。 实际上，即便此时查看状态信息，也会明白无误地看到关于重命名操作的说明：

    $ git mv README.md README
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
    renamed:    README.md -> README

其实，运行 git mv 就相当于运行了下面三条命令：

    $ mv README.md README
    $ git rm README.md
    $ git add README

如此分开操作，Git 也会意识到这是一次改名，所以不管何种方式结果都一样。 两者唯一的区别是，mv 是一条命令而另一种方式需要三条命令，直接用 `git mv` 轻便得多。 不过有时候用其他工具批处理改名的话，要记得在提交前删除老的文件名，再添加新的文件名。



##### **`忽略文件`**

一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 `.gitignore` 的文件，列出要忽略的文件模式。 来看一个实际的例子：

    $ cat .gitignore
    *.[oa]
    *~

第一行告诉 Git 忽略所有以 .o 或 .a 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的。 第二行告诉 Git 忽略所有以波浪符（~）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件名保存副本。 此外，你可能还需要忽略 log，tmp 或者 pid 目录，以及自动生成的文档等等。 要养成一开始就设置好 `.gitignore` 文件的习惯，以免将来误提交这类无用的文件。

文件 `.gitignore` 的格式规范如下：

    所有空行或者以 ＃ 开头的行都会被 Git 忽略。
    
    可以使用标准的 glob 模式匹配。
    
    匹配模式可以以（/）开头防止递归。
    
    匹配模式可以以（/）结尾指定目录。
    
    要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

> 所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 使用两个星号（*) 表示匹配任意中间目录，比如a/**/z 可以匹配 a/z, a/b/z 或 a/b/c/z等。

我们再看一个 .gitignore 文件的例子：
    
    # no .a files
    *.a
    
    # but do track lib.a, even though you're ignoring .a files above
    !lib.a
    
    # only ignore the TODO file in the current directory, not subdir/TODO
    /TODO
    
    # ignore all files in the build/ directory
    build/
    
    # ignore doc/notes.txt, but not doc/server/arch.txt
    doc/*.txt
    
    # ignore all .pdf files in the doc/ directory
    doc/**/*.pdf

GitHub 有一个十分详细的针对数十种项目及语言的 `.gitignore` 文件列表，你可以在 https://github.com/github/gitignore 找到它.
