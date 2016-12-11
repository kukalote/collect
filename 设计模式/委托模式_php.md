# 委托模式_php

<!-- create time: 2016-07-28 21:59:33  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

**名称 : 委托**

    通过分配或委托至其他对象, 委托设计模式能够去除核心对象中的判决和复杂的功能性。
    
委托模式致力于从核心对象中去除复杂性。此时我们并不设计极大依赖于通过评估条件语句而执行特定功能性的对象, 基于委托模式的对象能够将判决委托给不同的对象。委托既可以像使用中间对象处理判决树一样简单, 也可以像使用动态实例化对象提供期望的功能一样复杂。

    class Playlist
    {
        private $__songs;
        public fucntion __construct()
        {
            $this->__songs = array();
        }
        
        public function addSong($location, $title)
        {
            $song = array('location'=>$location , 'title'=>$title);
            $this->__songs[] = $song;
        }
        
        public function getM3U()
        {
            $m3u = "#EXTM3U\n\n";
            
            foreach($this->__songs as $song)
            {
                $m3u .= "#EXTINF:-1, {$song['title']}\n";
                $m3u .= "{$song['location']}\n";
            }
            return $m3u;
        }
        
        public function getPLS()
        {
            $pls = "[playlist]\nNumberOfEntries".count($this->__songs)."\n\n";
            
            foreach($this->__songs as $songCount=>$song)
            {
                $counter = $songCount + 1;
                $pls .= "File{$counter}={$song['location']}\n";
                $pls .= "Title{$counter}={$song['title']}\n";
                $pls .= "Length{$counter}=-1\n\n";
            }
            return $pls;
        }
    }
    
    $playlist = new Playlist();
    $playlist->addSong('/home/aaron/music/brr.mp3', 'Brr');
    $playlist->addSong('/home/aaron/music/goodbye.mp3', 'Goodbye');
    
    if($externalRetrievedType == 'pls')
    {
        $playlistContent = $playlist->getPLS();
    }
    else
    {
        $playlistContent = $playlist->getM3U();
    }
    
创建 newPlaylist 类是因为意识到要使用委托设计模式的事实。PHP 动态创建基于某个亦是的类实例的能力也十分有用。

    class newPlaylist
    {
        private $__songs;
        private $__typeObject;
        
        public function __construct($type)
        {
            $this->__songs = array();
            $object = "{$type}Playlist";
            $this->__typeObject = new $object;
        }
        
        public function addSong($location, $title)
        {
            $song = array('location'=>$location, 'title'=>$title);
            $this->__songs[] = $song;
        }
        
        public function getPlaylist()
        {
            $playlist = $this->__typeObject->getPlaylist($this->__songs);
            return $playlist;
        }
    }
    
调整如下, 作为 Playlist 对象原有的上述两个方法被移动至这引起对象自己的委托对象 : 

    class m3uPlaylistDelegate
    {
        public function getPlalist($songs)
        {
            $m3u = "#EXTM3U\n\n";
            
            foreach($songs as $song)
            {
                $m3u .= "#EXTINF:-1, {$song['title']}\n";
                $m3u .= "{$song['location']}\n";
            }
            return $m3u;
        }
    }
    
    class plsPlaylistDelegate
    {
        public function getPlaylist($songs)
        {
            $pls = "[playlist]\nNumberOfEntries=" . count($songs) . "\n\n";
            
            foreach( $songs as $songCount=>$song)
            {
                $counter = $songCount + 1;
                $pls .= "File{$counter}={$song['location']}\n";
                $pls .= "Title{$counter}={$song['title']}\n";
                $pls .= "Length{$counter}=-1\n\n";
            }
            
            return $$pls;
        }
    }
    
    $externalRetrievedType = 'pls';
    
    $playlist = new newPlaylist($externalRetrievedType);
    $playlistContent = $playlist->getPlaylist();
    
当通告其他播放列表格式时, 开发人员不必修改上面的代码就能创建基于委托设计模式的新类。

为了去除核心对象的复杂性并且能够动态添加新的功能, 就应当使用委托设计模式。