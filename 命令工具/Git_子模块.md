# Git_子模块

摘自 : https://git-scm.com/book/zh/v2/Git-工具-子模块

## 子模块

有种情况我们经常会遇到：某个工作中的项目需要包含并使用另一个项目。 也许是第三方库，或者你独立开发的，用于多个父项目的库。 现在问题来了：你想要把它们当做两个独立的项目，同时又想在一个项目中使用另一个。

Git 通过子模块来解决这个问题。 子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。 它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

## 开始使用子模块

我们首先将一个已存在的 Git 仓库添加为正在工作的仓库的子模块。 你可以通过在 `git submodule add` 命令后面加上想要跟踪的项目 URL 来添加新的子模块。 在本例中，我们将会添加一个名为 “**DbConnector**” 的库。

	$ git submodule add https://github.com/chaconinc/DbConnector

默认情况下，子模块会将子项目放到一个与仓库同名的目录中，本例中是 “DbConnector”。 如果你想要放到其他地方，那么可以在命令结尾添加一个不同的路径。

如果这时运行 `git status`，你会注意到几件事。

	$ git status
		new file:   .gitmodules
		new file:   DbConnector

	$ cat .gitmodules
	[submodule "DbConnector"]
		path = DbConnector
		url = https://github.com/chaconinc/DbConnector

如果有多个子模块，该文件中就会有多条记录。 要重点注意的是，该文件也像 `.gitignore` 文件一样受到（通过）版本控制。 它会和该项目的其他部分一同被拉取推送。 这就是克隆该项目的人知道去哪获得子模块的原因。

> 由于 `.gitmodules` 文件中的 URL 是人们首先尝试克隆/拉取的地方，因此请尽可能确保你使用的URL 大家都能访问。 例如，若你要使用的推送 URL 与他人的拉取 URL 不同，那么请使用他人能访问到的 URL。 你也可以根据自己的需要，通过在本地执行 `git config submodule.DbConnector.url <私有URL>` 来覆盖这个选项的值。 如果可行的话，一个相对路径会很有帮助。

虽然 **DbConnector** 是工作目录中的一个子目录，但 Git 还是会将它视作一个子模块。当你不在那个目录中时，Git 并不会跟踪它的内容， 而是将它看作该仓库中的一个特殊提交。

如果你想看到更漂亮的差异输出，可以给 **git diff** 传递 `--submodule` 选项。

	$ git diff --cached --submodule

当你提交时，会看到类似下面的信息：

	$ git commit -am 'added DbConnector module'
	[master fb9093c] added DbConnector module
	 2 files changed, 4 insertions(+)
	 create mode 100644 .gitmodules
	 create mode 160000 DbConnector

注意 **DbConnector** 记录的 **160000** 模式。 这是 Git 中的一种特殊模式，它本质上意味着你是将一次提交记作一项目录记录的，而非将它记录成一个子目录或者一个文件。

## 克隆含有子模块的项目

接下来我们将会克隆一个含有子模块的项目。 当你在克隆这样的项目时，默认会包含该子模块目录，但其中还没有任何文件：

	$ git clone https://github.com/chaconinc/MainProject
	$ cd MainProject
	$ cd DbConnector/
	$ ls
	$	//空文件夹

其中有 **DbConnector** 目录，不过是空的。 你必须运行两个命令：`git submodule init` 用来初始化本地配置文件，而 `git submodule update` 则从该项目中抓取所有数据并检出父项目中列出的合适的提交。

现在 **DbConnector** 子目录是处在和之前提交时相同的状态了。

不过还有更简单一点的方式。 如果给 **git clone** 命令传递 `--recursive` 选项，它就会自动初始化并更新仓库中的每一个子模块。

	$ git clone --recursive https://github.com/chaconinc/MainProject

## 在包含子模块的项目上工作

现在我们有一份包含子模块的项目副本，我们将会同时在主项目和子模块项目上与队员协作。

### 1. 拉取上游修改

在项目中使用子模块的最简模型，就是只使用子项目并不时地获取更新，而并不在你的检出中进行任何更改。 我们来看一个简单的例子。

如果想要在子模块中查看新工作，可以进入到目录中运行 **git fetch** 与 **git merge**，合并上游分支来更新本地代码。

	$ cd DbConnector
	$ git fetch	//获取子模块数据
	$ git merge origin/master	//合并远程子模块信息到本地

> **fetch** 会到远程仓库中拉取所有你本地仓库中还没有的数据。运行完成后，你就可以在本地访问该远程仓库中的所有分支，将其中某个分支合并到本地，或者只是取出某个分支，一探究竟。
>
> 如果是克隆了一个仓库，此命令会自动将远程仓库归于 **origin** 名下。所以，**git fetch origin** 会抓取从你上次克隆以来别人上传到此远程仓库中的所有更新（或是上次 **fetch** 以来别人提交的更新）。有一点很重要，需要记住，**fetch** 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支，只有当你确实准备好了，才能手工合并。
>
> 如果设置了某个分支用于跟踪某个远端仓库的分支， 可以使用 **git pull** 命令自动抓取数据下来， 然后将远端分支自动合并到本地仓库中当前分支。 在日常工作中我们经常这么用， 既快且好。 实际上， 默认情况下 **git clone** 命令本质上就是自动创建了本地的 **master** 分支用于跟踪远程仓库中的 **master** 分支（假设远程仓库确实有 **master** 分支）。 所以一般我们运行 **git pull**， 目的都是要从原始克隆的远端仓库中抓取数据后， 合并到工作目录中的当前分支。

如果你现在返回到主项目并运行 `git diff --submodule`，就会看到子模块被更新的同时获得了一个包含新添加提交的列表。 如果你不想每次运行 **git diff** 时都输入 `--submodle`，那么可以将 **diff.submodule** 设置为 “**log**” 来将其作为默认行为。

	$ git config --global diff.submodule log
	$ git diff	//查看子模块更新文件
	Submodule DbConnector c3f01dc..d0354fc:
	  > more efficient db routine
	  > better connection routine

如果在此时提交，那么你会将子模块锁定为其他人更新时的新代码。

如果你不想在子目录中手动抓取与合并，那么还有种更容易的方式。 运行 `git submodule update --remote`，Git 将会进入子模块然后抓取并更新。

	$ git submodule update --remote DbConnector	//更新子模块内容

此命令默认会假定你想要更新并检出子模块仓库的 **master** 分支。 不过你也可以设置为想要的其他分支。 例如，你想要 **DbConnector** 子模块跟踪仓库的 “**stable**” 分支，那么既可以在 **.gitmodules** 文件中设置（这样其他人也可以跟踪它），也可以只在本地的 **.git/config** 文件中设置。 让我们在 **.gitmodules** 文件中设置它：

	$ git config -f .gitmodules submodule.DbConnector.branch stable

	$ git submodule update --remote

> 如果不用 `-f .gitmodules` 选项，那么它只会为你做修改。但是在仓库中保留跟踪信息更有意义一些，因为其他人也可以得到同样的效果。
>
> .gitmodule 内容调整如下 :
>
> [submodule "submoudule_test"]  
	path = DbConnector  
	url =  https://github.com/chaconinc/DbConnector  
	**branch = stable**

这时我们运行 **git status**，Git 会显示子模块中有 “新提交”。

如果你设置了配置选项 status.submodulesummary，Git 也会显示你的子模块的更改摘要：

	$ git config status.submodulesummary 1	//显示子模块更新文件
	$ git status

这时如果运行 **git diff**，可以看到我们修改了 **.gitmodules** 文件，同时还有几个已拉取的提交需要提交到我们自己的子模块项目中。

我们可以直接看到将要提交到子模块中的提交日志。 提交之后，你也可以运行 `git log -p` 查看这个信息。

	$ git log -p --submodule	//到子模块目录下可看文件调整内容

当运行 `git submodule update --remote` 时，Git 默认会尝试更新所有子模块，所以如果有很多子模块的话，你可以传递想要更新的子模块的名字。


### 2. 在子模块上工作

现在我们将通过一个例子来演示如何在子模块与主项目中同时做修改，以及如何同时提交与发布那些修改。

到目前为止，当我们运行 `git submodule update` 从子模块仓库中抓取修改时，Git 将会获得这些改动并更新子目录中的文件，但是会将子仓库留在一个称作 “**游离的 HEAD**” 的状态。 这意味着没有本地工作分支（例如 “**master**”）跟踪改动。 所以你做的任何改动都不会被跟踪。

为了将子模块设置得更容易进入并修改，你需要做两件事。 首先，进入每个子模块并检出其相应的工作分支。 接着，若你做了更改就需要告诉 Git 它该做什么，然后运行 `git submodule update --remote` 来从上游拉取新工作。 你可以选择将它们合并到你的本地工作中，也可以尝试将你的工作变基到新的更改上。

首先，让我们进入子模块目录然后检出一个分支。

	$ git checkout stable
	Switched to branch 'stable'

然后尝试用 “**merge**” 选项。 为了手动指定它，我们只需给 **update** 添加 `--merge` 选项即可。 这时我们将会看到服务器上的这个子模块有一个改动并且它被合并了进来。

	//在项目主目录运行,从远程更新子模块代码
	$ git submodule update --remote --merge DbConnector

如果我们进入 DbConnector 目录，可以发现新的改动已经合并入本地 stable 分支。 现在让我们看看当我们对库做一些本地的改动而同时其他人推送另外一个修改到上游时会发生什么。

	$ cd DbConnector/
	$ vim src/db.c
	$ git commit -am 'unicode support'	//提交,待上传

如果我们现在更新子模块，就会看到当我们在本地做了更改时上游也有一个改动，我们需要将它并入本地。

	//回到主目录, 
	$ git submodule update --remote --rebase DbConnector

### 3. 发布子模块改动

现在我们的子模块目录中有一些改动。 其中有一些是我们通过更新从上游引入的，而另一些是本地生成的，由于我们还没有推送它们，所以对任何其他人都不可用。

Git 在推送到主项目前检查所有子模块是否已推送。 **git push** 命令接受可以设置为 “**check**” 或 “**on-demand**” 的 `--recurse-submodules` 参数。 如果任何提交的子模块改动没有推送那么 “**check**” 选项会直接使 **push** 操作失败。

	$ git push --recurse-submodules=check	//然而我这里没有操作成功, 推送前检查

如你所见，它也给我们了一些有用的建议，指导接下来该如何做。 最简单的选项是进入每一个子模块中然后手动推送到远程仓库，确保它们能被外部访问到，之后再次尝试这次推送。

另一个选项是使用 “**on-demand**” 值，它会尝试为你这样做。

	$ git push --recurse-submodules=on-demand	//我这里也没有操作成功,推送操作

Git 进入到 DbConnector 模块中然后在推送主项目前推送了它。 如果那个子模块因为某些原因推送失败，主项目也会推送失败。