# 外观模式_php

<!-- create time: 2016-07-28 23:30:05  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

**名称 : 外观**

    通过在必需的逻辑和方法的集合前创建简单的外观接口, 外观设计模式隐藏了来自调用对象的复杂性。
    
外观设计模式的目标是 : 控制外部错综复杂的关系, 并且提供简单的接口以利用上述组件的能力。外观设计模式的独特性在于被设计为将多个互相联系的组件组合或连接入简单可用的接口内。

为了隐藏复杂的、执行业务进程某个步骤所需的方法和逻辑组, 就应当使用基于外观设计模式的类。


    class CD
    {
        public $tracks = array();
        public $band = '';
        public $title = '';
        
        public function __construct($title, $band, $tracks)
        {
            $this->title = $title;
            $this->band = $band;
            $this->tracks = $tracks;
        }
    }
    
    $tracksFromExternalSource = array('What It Means', 'Brrr', 'Goodbye');
    $title = 'Waste of a Rib';
    $band = 'Never Again';
    $cd = new CD($title, $band, $tracksFromExternalSource);
    
注意到这两个类是为了最大可重用性而创建的是十分重要的。用户可能想将上述所有步骤组合入一个类, 但是以后您可能会被要求进行分解。

    class CDUpperCase
    {
        public static function makeString(CD $cd, $type)
        {
            $cd->$type = strtoupper($cd->$type);
        }
        
        public static function makeArray(CD $cd, $type)
        {
            $cd->$type = array_map('strtoupper', $cd->$type);
        }
    }
    
    class CDMakeXML
    {
        public static function create(CD $cd)
        {
            $doc = new DomDocument();
            $root = $doc->createElement('CD');
            $root = $doc->appendChild($root);
            
            $title = $doc->createElement('TITLE', $cd->title);
            $title = $root->appendChild($title);
            
            $band = $doc->createElement('BAND', $cd->band);
            $band = $root->appendChild($band);
            
            $tracks = $doc->createElement('TRACKS');
            $tracks = $root->appendChild($tracks);
            
            foreach($cd->tracks as $track)
            {
                $track = $doc->createElement('TRACK', $track);
                $track = $tracks->appendChild($track);
            }
            
            return $doc->saveXML();
        }
    }
    
    
    CDUpperCase::makeString($cd, 'title');
    CDUpperCase::makeString($cd, 'band');
    CDUpperCase::makeArray($cd, 'tracks');
    print CDMakeXML::create($cd);

我们应当针对具体的 Web 服务调用创建一个对象 : 

    class WebServiceFacade
    {
        public static function makeXMLCall(CD $cd)
        {
            CDUpperCase::makeString($cd, 'title');
            CDUpperCase::makeString($cd, 'band');
            CDUpperCase::makeArray($cd, 'tracks');
            $xml = CDMakeXML::create($cd);
            return $xml;
        }
    }
    
    print WebServiceFacade::makeXMLCall($cd);
    
在应用程序进程中的下一步骤包含许多复杂的逻辑步骤和方法调用时, 最佳的做法是创建一个基于外观设计模式的对象。