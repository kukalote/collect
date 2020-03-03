### PHP_将数据编码后存储

如果将`数组序列化`或者将`链接编码`后存储到数据库, 可能会有一些特殊字符被转义, 当然如果除了这样的问题, 还有安全问题。

所以这里我们最好将接收到的数据先转成安全的字符串后再存储。

**`Tips : `** 这里没有介绍关于XSS注入之类的注入脚本的攻击, 主要是对数据库安全和保存数据完整性的讨论。

下面是用到的编码的一种, 以保存数组为例 : 

    <?php
        $arr = array(xxxx);
        //加密过程
        $arr_serial = serialize($arr);
        $str_encode = base64_encode($arr_serial);
        
        //解密过程
        $arr_serial = base64_decode($str_encode);
        $arr = unserialize($arr_serial);
        