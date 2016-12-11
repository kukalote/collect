### Linux_shell 批量重命名或转移

`rename` 命令利用 Perl 正则表达式修改文件名。综合运用 `find`、`rename` 和 `mv` , 我们就可以做到批量重命令或转移位置了。

#### 展示示例

- 将 *.JPG 更名为 *.jpg : 

        $ rename *.JPG *.jpg
        
- 将文件名中的空格替换成字符 "-" : 

        $ rename 's/ /_/g' * 
        
- 转换文件名大小写 : 

        $ rename 'y/A-Z/a-z/' *
        $ rename 'y/a-z/A-Z/' * 
        
- 将所有的 .mp3 文件移入给定的目录 : 

        $ find path -type f -name "*.mp3" -exec mv {} target_dir \;
        
- 将所有文件名中的空格替换为字符 "-" : 

        $ find path -type f -exec rename 's/ /_/g' {} \;