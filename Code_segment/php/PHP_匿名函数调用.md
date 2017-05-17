### PHP 匿名函数调用  



#### 访问匿名函数  
```
//通过方法访问匿名方法(1)
function anonyFunc($num)
{
    $count = 1;
	var_dump($num);
	return function ($str)use( $num, & $count )
	{
	    $count += 1;
		var_dump($str, $count);
	};
}

call_user_func(anonyFunc(1024), '调用匿名函数！');
/*
int(1024)
string(21) "调用匿名函数！"
int(2)
*/
```  

如果我们放到 **call_user_func** 中的是方法可以直接将返回的匿名函数当作 `参数1` ,如果传入的`参数1`只是方法名，那我们就只好调用这个方法本身，因为这时 **call_user_func** 返回的才是匿名函数体。
  
```
//通过方法访问匿名方法(2)
function anonyFunc($num)
{
    $count = 1;
	var_dump($num);
	return function ($str)use( $num, & $count )
	{
	    $count += 1;
		var_dump($str, $count);
	};
}

$func = call_user_func('anonyFunc', 1024);
//$func('匿名方法!');
/*
int(1024)
//string(21) "调用匿名函数！"
//int(2)
*/
```

#### 类通过对象访问匿名函数  
这里用于与下面一个调用方法的区别，所以执行了两次。注意这里调用的方法是用对象调用的，`$count` 输出时都是*2*    

```  
class Test1
{
	public function anonyFunc($num)
	{
	    $count = 1;
		var_dump($num);
		return function ($str)use( $num, & $count )    //这里 $count 用的是指针
		{
		    $count += 1;
			var_dump($str, $count);
		};
	}
}

$obj = new Test1();
call_user_func($obj->anonyFunc(1024), '调用匿名函数！');
call_user_func($obj->anonyFunc(1024), '调用匿名函数！');
/*
int(1024)
string(21) "调用匿名函数！"
int(2)

int(1024)
string(21) "调用匿名函数！"
int(2)

*/
```

#### 类通过静态方法访问匿名函数  
这里用于与下面一个调用方法的区别，所以执行了两次。注意这里调用的方法是用对象调用的，`$count` 输出时都是*2*    

```  
class Test2
{
	public static function anonyFunc($num)
	{
	    $count = 1;
		var_dump($num);
		return function ($str)use( $num, & $count )    //这里 $count 用的是指针
		{
		    $count += 1;
			var_dump($str, $count);
		};
	}
}

call_user_func( Test2::anonyFunc(1024), '调用匿名函数！');
call_user_func( Test2::anonyFunc(1024), '调用匿名函数！');
/*
int(1024)
string(21) "调用匿名函数！"
int(2)

int(1024)
string(21) "调用匿名函数！"
int(2)

*/
```
#### 调用类中匿名函数
这里也是只调用类中方法，并未用到匿名方法来处理数据，除非额外调用

```  
class Test2
{
	public function anonyFunc($num)
	{
	    $count = 1;
		var_dump($num);
		return function ($str)use( $num, & $count )    //这里 $count 用的是指针
		{
		    $count += 1;
			var_dump($str, $count);
		};
	}
}

$func = call_user_func( array(new Test3(), 'anonyFunc'), 1024);
//$func('调用匿名函数!');
/*
int(1024)
//string(21) "调用匿名函数！"
//int(2)
*/
```

#### 回调匿名函数

```  
class ProcessSale
{
    private $callbacks;
    function registerCallback($callback)
    {
        if( ! is_callable($callback))
        {
            throw new Exception("callback not callable");
        }
        $this->callbacks[] = $callback;
    }
    function sale ( $product)
    {
        foreach( $this->callbacks as $callback)
        {
            call_user_func( $callback, $product);
        }
    }
}
$processor = new ProcessSale();
$processor->registerCallback( Test2::anonyFunc(1024) );
$processor->sale('调用匿名函数1！');
$processor->sale('调用匿名函数2！');
/*
int(1024)                    //这里只调用了一次哦
string(22) "调用匿名函数1！"
int(2)                       //这里是2
string(22) "调用匿名函数2！"
int(3)                       //这里变成3了，是不是说明：静态调用方法是常驻内存，其变量也是累加的？
*/

$obj = new Test1();
$processor->registerCallback( $obj->anonyFunc(1024) );
$processor->sale('调用匿名函数1！');
$processor->sale('调用匿名函数2！');
/*
int(1024)                    //这里只调用了一次哦
string(22) "调用匿名函数1！"
int(2)                       //这里是2
string(22) "调用匿名函数2！"
int(3)                       //这里变成3了，看来，不是静态方法的原因，本来我们将匿名方法就是记录到内存中了
*/


```






