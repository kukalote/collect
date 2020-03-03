# 适配器模式_php

<!-- create time: 2016-07-26 22:33:07  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

**名称：适配器**

    适配器设计模式只是将某个对象的接口适配为另一个对象所期望的接口。


    class errorObject
    {
        private $_error;
        public function __construct($error)
        {
            $this->_error = $error;
        }
        
        public function getError()
        {
            return $this->_error;
        }
    }
    
    class logToConsole
    {
        private $_errorObject;
        pubilc function __construct($errorObject)
        {
            $this->_errorObject = $errorObject;
        }

        public function write()
        {
            fwrite(STDERR, $this->_errorObject->getError());
        }
    }
    
    $error = new errorObject("404:Not Found");
    
    $log = new logToConsole($error);
    $log->write();
    
将 `logToConsole` 改为如下方式表达 : 
    
    class logToCSV
    {
        const CSV_LOCATION = 'log.csv';
        private $_errorObject;
        public function __construct($errorObject)
        {
            $this->_errorObject = $errorObject;
        }
        
        public function write()
        {
            $line = $this->_errorObject->getErrorNumber();
            $line .= ',';
            $line .= $this->_errorObject->getErrorText();
            $line .= "\n";
            
            file_put_contents(self::CSV_LOVATION, $line, FILE_APPEND);
        }
    }
    
    class logToCSVAdapter extends errorObject
    {
        private $_errorNumber, $_errorText;
        public funciton __construct($error)
        {
            parent::__construct($error);
            
            $parts = explede(':', $this->getError());
            
            $this->_errorNumber = $parts[0];
            $this->_errorText = $parts[1];
        }
        
        public funciton getErrorNumber()
        {
            return $this->_errorNumber;
        }
        
        public function getErrorText()
        {
            return $this->_errorText;
        }
    }
    
    $error = new logToCSVAdapter("404:Not Found");
    
    $log = new logToCSV($error);
    $log->write();