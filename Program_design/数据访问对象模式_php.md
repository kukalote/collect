# 数据访问对象模式_php

<!-- create time: 2016-07-27 22:10:17  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->
**名称 : 数据访问对象**

    数据访问对象设计模式描述了如何创建提供透明访问任何数据源的对象。


处理引用特定数据库信息的实体时, 最好的做法是创建一个数据访问对象。

    abstract class baseDAO
    {
        private $__connection;
        
        public function __construct()
        {
            $this->__connectToDB(DB_USER, DB_PASS, DB_HOST, DB_DATABASE);
        }
        
        private function __connectToDB($user, $pass, $host, $database)
        {
            $this->__connection = mysql_connect($host, $uesr ,$pass);
            mysql_select_db($database, $this->__connection);
        }
        
        public function fetch($value, $key=NULL)
        {
            if(is_null($key))
            {
                $key = $this->_primaryKey;
            }
            
            $sql = "select * from {$this->_tableName} where {$key}='{$value}' ";
            $results = mysql_query($sql, $this->__connection);
            
            $rows = array();
            while($result = mysql_fetch_array($results))
            {
                $rows[] = $result;
            }
            
            return $rows;
        }
        
        public function update($keyedArray)
        {
            $sql = "update {$this->_tableName} set ";
            
            $updates = array();
            foreach($keyedArray as $column=>$value)
            {
                $updates[] = "{$column}='{$value}' ";
            }
            
            $sql .= implode(',' , $updates);
            $sql .= "where {$this->_primaryKey}='{$keyedArray[$this->_primaryKey]}'";
            
            mysql_query($sql, $this->__connection);
        }
    }
    
    class userDAO extends baseDAO
    {
        protected $_tableName = 'userTable';
        protected $_primaryKey = 'id';
        
        public function getUserByFirstName($name)
        {
            $result = $this->fetch($name, 'firstName');
            return $result;
        }
    }
    
    define('DB_USER', 'user');
    define('DB_PASS', 'pass');
    define('DB_HOST', 'localhost');
    define('DB_DATABASE', 'test');
    
    $user = new userDAO();
    $userDetailsArray = $user->fetch(1);
    
    $updates = array('id'=>1, 'firstName'=>'aaron');
    $user->update($updates);
    
    $allAarons = $user->getUserByFirstName('aaron');
    
为了减少重复和抽象化数据, 最好的做法是基于数据访问对象创建一个类。