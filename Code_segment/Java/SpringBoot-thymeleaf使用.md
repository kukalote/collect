# thymeleaf使用

### 1、@EnableWebMvc 全面接管SpringMVC自动配置，所有SpringMVC自动配置都将失效。

```java

@EnableWebMvc
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

```

### 2、国际化

设置国际化配置文件

![image-20211012203356578](.\images\image-20211012203356578.png)

设置 `application.properties`:

```java
spring.messages.basename=i18n.login
```

thymeleaf 中获取国际化词汇使用方法为：

```java
<div th:text="#{login.tip}"></div>
```

### 3、拦截器

设置自定义拦截器

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object username = request.getSession().getAttribute("username");
        System.out.println(username);
        if (username == null) {
            request.setAttribute("msg", "没有权限，请先登陆！");
            request.getRequestDispatcher("/login").forward(request, response);
            return false;
        } else {
            return true;
        }
    }
```

创建拦截器后，还要注册到容器中：

```java
@Bean
public WebMvcConfigurer webMvcConfigurer() {
    return new WebMvcConfigurer() {
        @Override
        public void addViewControllers(ViewControllerRegistry registry) {
            // 拦截器相关
            registry.addViewController("/login").setViewName("login");
            registry.addViewController("/loginFail").setViewName("fail");
        }

        // 注册拦截器
        @Override
        public void addInterceptors(InterceptorRegistry registry) {
            registry.addInterceptor(new LoginHandlerInterceptor())
                .addPathPatterns("/loginSuccess");
//                  .excludePathPatterns("/**");
        }
    };
}
```

### 4、