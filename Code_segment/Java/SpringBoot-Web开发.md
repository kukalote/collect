# Web开发

### 1、"/**"访问当前项目的任何资源， 静态资源文件夹

> classpath:/META-INF/resources
>
> classpath:/resources,
>
> classpath:/static/
>
> classpath:/public/
>
> /:当前项目的根路径

> **注：**模板引擎只对resources文件夹或自定义文件夹下的文件起作用
### 2、图标设置

**/favicon.ico

### 3、静态资源设置

```properties
# 设置静态访问地址（之前默认不可用）
spring.resources.static-locations=classpath:/xxx/,classpath:/yyy/
```

### 4、错误处理

错误页面放置位置：resources/error/状态码.html, `4xx.html`和`5xx.html`可以概括 400通用状态码和500通用状态码。

如：

```
resources/error/404.html
resources/error/5xx.html
```

