#IP与整型之间的转换

这里讲的主要是 `ip4` 的转换， `ip6` 的可依此类推。

##1. 把IP转换为整型

我们在这里可以把 `ip4` , 分为 4 段， 每一段都有自己 `0-255` (共 256 种变化)。如果把这 4 段当作 256 进制的整型来处理， 保存为十进制数字，如下 :

```
	<?php
	$ip = '127.0.0.1';
	$position = 3;
	$long = 0;
	$iparr = explode( '.', $ip );
	foreach( $iparr as $ipceil ) {
		$long += $ipceil * pow( 256, $position-- );
	}
	var_dump($long);exit;
```

##2. 把整型回转为IP

上面已经知道把 `ip` 转换为整型的逻辑，反转只是求余数与求除数的手法了。

```
	<?php
	$long = 2130706433;
	$ipstr = '';
	$range = 0;
	for( $i=3; $i>0; $i--) {
		$range = pow( 256, $i );
		$ipceil = floor( $long/$range );
		$ipstr .= "{$ipceil}.";
		$long   = $long%$range;
	}
	$ipstr .= $long;
```

##3. MYSQL 中IP字符串转整型 与反转型

	select INET_ATON ('127.0.0.1');		//2130706433
	select INET_NTOA ('2130706433');	//127.0.0.1