### Linux_bash 删除重复文件的脚本

    #!/bin/bash
    # 文件名 : remove_duplicates.sh
    # 用途 : 查找并删除重复文件, 每一个文件只保留一份
    
    ls -lS --time-style=long-iso | awk 'BEGIN {
        getline; getline;    #第一行是文件数删除，第二行作为初始文件进行比较
        name1=$8; size=$5;   #初始文件赋值
    }
    {
        name2=$8;            #第二个文件赋值
        if (size==$5)
        {
            "md5sum "name1 | getline; csum1=$1;    #取当前行的 md5值
            "md5sum "name2 | getline; csum2=$1;
            if (csum1==csum2)
            {
                print name1; print name2;
            }
        };
        
        size=$5; name1=name2;    #进行下一轮比较的初始化
    }' | sort -u > duplicate_files
    
    # 对文件生成32位md5码，跟据前32位选出惟一，并把惟一文件名保存
    cat duplicate_files | xargs -I {} md5sum {} | sort | uniq -w 32 | awk '{ print "^"$2"$" }' | sort -u > duplicate_sample
    
    echo Removing..
    #对duplicate_files文件差集进行删除
    comm duplicate_files duplicate_sample -2 -3 | tee /dev/stderr | xargs rm
    echo Removed duplicates files successfully.