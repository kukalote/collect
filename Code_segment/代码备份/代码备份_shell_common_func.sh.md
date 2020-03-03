### 代码备份_shell common_func.sh

    # my script functions
    . ~/Documents/language_bash/common_func/alias_command.bash
    
    ###############################
    #########公共/工具方法#########
    ###############################
    
    # 测试进程是否运行
    function hasPS {
        info=`ps -x | grep $1 | grep -v grep`
    
        if [ $? -eq 0 ]
        then
            return 0
        else
            return 1
        fi
    }
    
    # 是否存在目录，如果不存在就创建
    function hasDir {
        if [ -d $1 ]
        then
            return 0
        else
            mkdir -p $1
            return $?
        fi
    
    }
    #mysql_dev
    
    
    
    
    function mysql_dev {
        MYSQL=`which mysql`
        hasPS mysqld
        if [ $? -ne 0 ]
        then
            echo '未开启mysql 服务'
        fi
    
        $MYSQL -h 10.207.26.144 -u root -proot
    }
    
    # 伪删除
    function bkrm {
        rmDirByDay 7
        param_count=${#@}
        if [ $param_count -lt 1 ]
        then
            \rm $@
        else
            date=`date +%Y%m%d%w`
            bak_dir="/Users/xun/Documents/backup/$date"
            is_dir=`hasDir $bak_dir`
            if [ $? -eq 0 ]
            then
                echo "将文件移到$bak_dir, 一周后再删除"
            fi
            mv $@ $bak_dir  #将rm 参数添加到mv命令中
        fi
        #cp $@ $bak_dir
        #rm $@
    }
    
    # 删除以数字为基数的文件夹或目录
    function rmDirByDay {
        cycle=$1
    #
    #   rm_date=$(date -v-{$cycle}d +%Y%m%d%w)
    #   bak_dir="/Users/xun/Documents/backup/$rm_date"
    #   if [ -d $bak_dir ]
    #   then
    #       rm -R $bak_dir
    #   fi
    #   rm_date=$(date -v -{$cycle}d +%Y%m%d%w)
    
        bak_dir="/Users/xun/Documents/backup"
        result=$( find $bak_dir -maxdepth 1 -mindepth 1 -type d -ctime +$cycle -print0 | xargs -0 );
        #result=$( find $bak_dir -maxdepth 1 -mindepth 1 -type d -ctime +$cycle -print0 | xargs -0 \rm -R );
        echo "\rm -R $result"
        #result=$(\rm -R $result)
        return 0;
    }
