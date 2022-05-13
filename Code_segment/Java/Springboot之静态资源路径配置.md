# Springboot之静态资源路径配置

在 Springboot 中可以直接在配置文件中覆盖默认的静态资源路径的配置信息：

- `application.properties`配置文件如下

```java
server.port=1122

web.upload-path=/temp/study13/

spring.mvc.static-path-pattern=/**
spring.resources.static-locations=classpath:/META-INF/resources/,classpath:/resources/,\
  classpath:/static/,classpath:/public/,file:${web.upload-path}
```

字段名 | 值（示例） | 含义
---|---|---
`web.upload-path`|/temp/study13/|指定了一个上传文件保存路径，注意要以`/`结尾
`spring.mvc.static-path-pattern`|/**|表示所有的访问都经过静态资源路径
`spring.resources.static-locations`|classpath:/META-INF/resources/|在这里配置静态资源路径

**注意：**
`spring.resources.static-locations`在这里配置静态资源路径，前面说了这里的配置是覆盖默认配置，所以需要将默认的也加上否则`static`、`public`等这些路径将不能被当作静态资源路径，在这个最末尾的`file:${web.upload-path}`之所有要加`file:`是因为指定的是一个具体的硬盘路径，其他的使用`classpath`指的是系统环境变量