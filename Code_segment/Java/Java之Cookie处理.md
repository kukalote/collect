# Java之Cookie处理



### Controller中的Cookies操作

```java
public class CookieUtils {
	// 设置 cookies
    public static void set(HttpServletResponse response, String name, String value, int maxAge) {
        Cookie cookie = new Cookie(name, value);
        cookie.setPath("/");
        cookie.setMaxAge(maxAge);
        response.addCookie(cookie);
    }

    // 查找 cookies
    public static Cookie get(HttpServletRequest request, String name) {
        //1.将cookies放到map中去
        Map<String, Cookie> cookieMap = new HashMap<>();
        Cookie[] cookies = request.getCookies();
        if (cookies != null) {
            for (Cookie cookie : cookies) {
                cookieMap.put(cookie.getName(), cookie);
            }
        }
        //2.查找是否存在cookie,是则返回查找到的cookie
        if (cookieMap.containsKey(name)) {
            return cookieMap.get(name);
        } else {
            return null;
        }
    }
}

// 销毁 cookies
CookieUtils.set(response, "key", null, 0);
```



### 操作Cookies

```java
// 生成 cookie
Cookie cookie = new Cookie(name, value);
// 设置保存地址
cookie.setPath("/");
// 设置保存时长
cookie.setMaxAge(maxAge);

// controller 中获取cookies
Cookie[] cookies = request.getCookies();
for (Cookie cookie : cookies) {
    cookie.getName();	// cookies名
    cookie.getValue();  // cookies值
}
```



### 保存\读取Json

```java
// 保存 json
String jsonValue = URLEncoder.encode(jsonStr, "utf-8");
cookie.put(key, jsonValue);

// 读取 json
String jsonValue = cookie.getValue();
String jsonStr = URLDecoder.decode(jsonValue, "utf-8");
```

