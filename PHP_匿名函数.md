### PHP_匿名函数

1. 如果没有传参或用**use()**，方法内部是无法调取参数 ： 
```php

	$message = 'hello';

	// 没有 "use"
	$example = function () {
	    var_dump($message);
	};
	echo $example();
```
	Notice: Undefined variable: message in /var/www/index.php on line 7
	NULL
2. 使用**use()**给方法传参 : 
```php

	$message = 'hello';
	// 继承 $message
	$example = function () use ($message) {
	    var_dump($message);
	};
	echo $example();
```

	string(5) "hello"
3.  匿名函数的参数是在声明时候就调用，后面变量更新时结果不变 : 
```php

	$message = 'hello';
	// 继承 $message
	$example = function () use ($message) {
	    var_dump($message);
	};
	// Inherited variable's value is from when the function
	// is defined, not when called
	$message = 'world';
	echo $example();
```
	string(5)"hello"
4. 匿名函数如果是引址调用, 后面变量更新则结果将随着调整 :  
```php

	// Reset message
	$message = 'hello';

	// Inherit by-reference
	$example = function () use (&$message) {
	    var_dump($message);
	};
	echo $example();

	// The changed value in the parent scope
	// is reflected inside the function call
	$message = 'world';
	echo $example();
```
	string(5) "hello"
	string(5) "world"
5.  匿名函数可以传分两部分传递参数 : 
```php

	$message = 'world';
	// Closures can also accept regular arguments
	$example = function ($arg) use ($message) {
	    var_dump($arg . ' ' . $message);
	};
	$example("hello");
	//这里 $arg 是常规方法参数传递方式
	//而 $message 是从上级域中直接放的参数
```
	string(11) "hello world"
6. 还可以进行匿名函数嵌套 : 
```php

	function anonymous()
	{
	    return function ($a, $b) 
	    {   
	        return function ($c) use ($a, $b) 
	        {   
	            var_dump($a, $b, $c);
	        };  
	    };  
	}
	$t = anonymous();
	
	$g = $t('a', 'b');
	$g('c');
```

	string(1) "a" string(1) "b" string(1) "c"