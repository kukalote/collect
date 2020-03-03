# 建造者模式_php

<!-- create time: 2016-07-27 00:00:04  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

**名称 : 建造者**

    建造者设计模式定义了处理其他对象的复杂构建的对象设计。
    
原型如下 : 

    class product
    {
        protected $_type = '';
        protected $_size = '';
        protected $_color = '';
        
        public function setType($type)
        {
            $this->_type = $type;
        }
        
        public function setSize($size)
        {
            $this->_size = $size;
        }
        
        public function setColor($color)
        {
            $this->_color = $color;
        }
    }
    
    $productConfigs = array('type'=>'shirt', 'size'=>'XL', 'color'=>'red');
    
    $product = new product();
    $product->setType($productConfigs['type']);
    $product->setSize($productConfigs['size']);
    $product->setColor($productConfigs['color']);
    
创建对象时调用每个方法并不是最佳的。调整如下 : 

    class productBuilder
    {
        protected $_product = NULL;
        protected $_configs = array();
        
        public funciton __construct($configs)
        {
            $this->_product = new product();
            $this->_xml = $configs;
        }
        
        public function build()
        {
            $this->_product->setSize($configs['size']);
            $this->_product->setType($configs['type']);
            $this->_product->setColor($configs['color']);
        }
        
        public function getProduct()
        {
           return $this->_product; 
        }
    }
    
    $builder = new productBuilder($productConfigs);
    $builder->build();
    $product = $builder->getProduct();
    
> 建造者模式的目的是消除其他对象的复杂创建过程。使用建造者设计模式不仅是最佳的做法， 而且在某个对象的构造和配置方法改变时可以尽量可能地减少重复更改代码。