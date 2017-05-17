### Mac 快捷键

功能 | 快捷键 
----|----
Mission | Ctrl + 向下键
隐藏Dock | Alt + Cmd + D
当前窗口最小化 | Cmd + M
当前窗口隐藏 | Cmd + H
除了当前窗口，其他都隐藏 | Alt + Cmd + H
显示软件下全部窗口 | Shift + Ctrl + 向下键
还原最小化窗口 | 按住Cmd + Tab激活程序切换 -> 松开Tab，按住Cmd不放的同时按住Opt（alt) -> 松开Cmd

### 终端组合键

组合键 | 说明
----|---
Ctrl + A | 命令行前端
Ctrl + E | 命令行后端
Ctrl + P | 前一个命令
Ctrl + N | 下一个命令
Ctrl + W | 删除前一个单词
Ctrl + U | 删除(剪切)命令
Ctrl + Y | 粘贴命令
Ctrl + D | 向后删除字符(相当于 delete 键)
Ctrl + H | 向前删除字符 ( 相当于 删除键 )
Ctrl + B | 向后 ( 左 ) 移动一字符
Ctrl + F | 向前 ( 右 ) 移动一字符
Ctrl + R | 搜索最近命令



### Mac 快捷操作
1. Finder和Terminal终端之间的集成是相互的，当你把Finder中的一个文件拖入到Terminal终端窗口时，它的绝对路径就会被粘贴在命令行中


### Mac 设置
1. 在Finder标题栏显示完整路径  
在“终端”中输入下面的命令：  
		
		defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES  
		killall Finder


### Mac OS X 命令行用户应当知道的八个终端工具
1. **open** :打开文件，目录和程序。  

		//打开浏览器
		$ open /Applications/Safari.app
2. **pbcopy** 和 **pbpaste** : 复制和粘贴命令行中的文本内容  

		//输入该命令时会将你的home目录中的文件列表拷贝到OS X 系统的剪切板中。
		$ ls ~ | pbcopy
		//你可以通过如下命令轻易的获取文档的内容：
		$ pbcopy > tasklist.txt  
3. **mdfind** : 任何 Spotlight 能搜索到的内容，mdfind 同样也能搜索到

        //-onlyin 标识可以将搜索限制在一个指定的目录中
        $ mdfind -onlyin ~/Documents essay   
mdfind数据库需要在后台经常更新数据，但你可以使用mdutil命令来排除故障（同样适用于Spotlight)。如果Spolight没有正确的工作，使用mdutil -E将会清楚数据库中的索引然后通过抓取重建索引。你同样也可以通过运行mdutil -i off 来完全完毕索引。  
4. **screencapture** : 抓图程序 或者cmd + shift +3 和 cmd + shift + 4 的截屏快捷键

        //抓取屏幕中的所有内容，包括光标，并且将该截图（以’image.png’命名）附再一封新的电子邮件中        
        $ screencapture -C -M image.png   
        //延时10秒截屏并且在预览中打开该截图    
        $ screencapture -T 10 -P image.png
        //通过你的鼠标选择一个窗口，然后抓取该窗口中的内容（不包括该窗口的阴影效果）将该截图复制到剪切板中
        $ screencapture -c -W   
        //通过鼠标选定一个区域截图，同时将该内容保存为pdf文件
        $ screencapture -s -t pdf image.pdf

5. **launchctl** : 可以让你与OS X 的初始化脚本系统launchd进行交互  
    通过启动守护进程与启动代理，你可以在启动你的电脑时控制你的启动服务项。你甚至可以通过编写脚本定期或再指定的时间间隔内执行操作，类似于Linux中的corn工具。    
        
        //当你想要在你启动你的Mac时自动启动Apache服务器时，你可以输入  
        $ sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist  
        //会列出所有当前状态下加载了的启动脚本  
        $ sudo launchctl list
        //停止并且卸载运行中的脚本，该命令增加 -w 标识时会将该脚本永久的从你的启动队列中清除。
        $ sudo launchctl unload [path/to/script] 
        
        //启动脚本储存在以下几个位置中：
        ~/Library/LaunchAgents         
        /Library/LaunchAgents            
        /Library/LaunchDaemons         
        /System/Library/LaunchAgents            
        /System/Library/LaunchDaemons  
 6. **say** : 可以将文本转换为语音  
     它使用了OS X中 VoiceOver 使用的文字语音转换系统。
     
         //无需任何选项，say命令会将你输入的任何文本内容转化为语音输出 
         $ say “Never trust a computer you can’t lift.”  
         //使用带-f 标识的say命令来朗读一个文本文档中的内容，同时使用-o 标识来保存输出的音频内容
         $ say -f mynovel.txt -o myaudiobook.aiff  
         
         say 命令可以用于脚本的控制台日志和报警声音。
7. **diskutil** : diskutil 是OS X中 磁盘管理工具 的命令行界面  
    它可以完成任何它的图形界面兄弟能完成的任务，同时它也包含一些额外的能力—例如在一个磁盘中写满零或者随机数据。简单的输入diskutil list会列出所有磁盘的路径名和链接到你电脑上的可移除的媒体介质，然后再指向你想要操作的卷的命令。请注意：如果不正确的使用diskutil命令会永久的清楚磁盘上的数据。  
    
    
    
	

	
  
  