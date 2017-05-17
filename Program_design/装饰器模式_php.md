# 装饰器模式_php

<!-- create time: 2016-07-27 23:16:41  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

**名称 : 装饰器**

    如果已有对象的部分内容或功能性发生改变, 但是不需要修改原始对象的结构, 
    那么使用装饰器设计模式最适合。
    
>装饰器设计模式适用于编程人员花费大量时间所处的下列工作场合 : 变化是快速和细小的, 而且几乎不影响应用程序的其余部分。

>使用装饰器设计模式类的目标是 : 不必重写任何已有的功能性, 而是对某个基对象应用增量变化。

>装饰器采用这样的构建方式 : 在主代码流中应当能够直接插入一个或多个更改或 "装饰" 目标对象的装饰器, 同时不影响其他代码流。


    class CD
    {
        public $trackList;
        public function __construct()
        {
            $this->trackList = array();
        }
        
        public function addTrack($track)
        {
            $this->trackList[] = $track;
        }
        
        public function getTrackList()
        {
            $output = '';
            
            foreach($this->trackList as $num=>$track)
            {
                $output .= ($num + 1) . ") {$track}. ";
            }
            return $output;
        }
    }
    
    $tracksFromExternalSource = array('What It Means', 'Brr', 'Goodbye');
    
    $myCD = new CD();
    
    foreach($tracksFromExternalSource as $track){
        $myCD->addTrack($track);
    }
    
    print "The CD contains the following tracks: " . $myCD->getTrackList();
    
对于该示例来说, 上述代码已经可以很好地解决问题了。不过, 此时需求又发生了小变化 : 只针对这个输出实例, 输出的每个音轨都需要采用大写形式。对这么小的变化而言, 最佳的做法并非修改基类或创建父-子关系, 而是创建一个基于装饰器设计模式的对象。

    
    class CDTrackListDecoratorCaps
    {
        private $__cd;
        public function __construct(CD $cd)
        {
            $this->__cd = $cd;
        }
        
        public function makeCaps()
        {
            foreach($this->__cd->trackList as & $track)
            {
                $track = strtoupper($track);
            }
        }
    }    
    
    $tracksFromExternalSource = array('What It Means', 'Brr', 'Goodbye');
    $myCD = new CD();
    
    foreach($tracksFromExternalSource as $track)
    {
        $myCD->addTrack($track);
    }
    
    $myCDCaps = new CDTrackListDecoratorCaps($myCD);
    $myCDCaps->makeCaps();
    
    print "The CD contains the following tracks: " . $myCD->getTrackList();
    

为了在不修改对象结构的前提下对现有对象的内容或功能性稍加修改, 就应当使用装饰器设计模式。