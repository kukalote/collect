# 动态事件绑定
从 jQuery 1.7 开始，不再建议使用 .live() 方法。请使用 .on() 来添加事件处理。使用旧版本的用户，应该优先使用 .delegate() 来替代 .live()。 

### on 的绑定与功能
```javascript
$("p").on("click", function(){
	alert( $(this).text() );
});
```

on() 功能 | 可以绑定一个或多个操作事件(不同事件以空格隔开)
---|---
**on() 不足** | 只是一次性的，无法动态绑定。

###  delegate 新动态方法的替代者

使用 delegate() 方法的事件处理程序适用于当前或未来的元素（比如由脚本创建的新元素）。
```javascript
$("body").delegate(".sub", "hover", function(){
	$(this).toggleClass("hover");
});

// 替换动态的 hover(function(){},function(){});
$("body").delegate(".jp-knob", "mouseenter mouseleave", function(event){ 
	if( event.type == "mouseenter" ) { 
		$(".jp-knob-pop", $(this)).hide()
	} else {
		$(".jp-knob-pop", $(this)).hide()
	}   
});


```

