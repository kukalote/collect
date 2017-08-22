# C`_getcwd_`获取当前工作目录

头文件：`#include <unistd.h>`

定义函数：`char * getcwd(char * buf, size_t size);`

函数说明：`getcwd()` 会将当前的工作目录绝对路径复制到参数buf 所指的内存空间，参数size 为buf 的空间大小。

注：
> 1. 在调用此函数时，buf 所指的内存空间要足够大。若工作目录绝对路径的字符串长度超过参数size 大小，则返回NULL，errno 的值则为ERANGE。
> 2. 倘若参数buf 为NULL，getcwd()会依参数size 的大小自动配置内存(使用malloc())，如果参数size 也为0，则getcwd()会依工作目录绝对路径的字符串程度来决定所配置的内存大小，进程可以在使用完次字符串后利用free()来释放此空间。

返回值：执行成功则将结果复制到参数buf 所指的内存空间, 或是返回自动配置的字符串指针. 失败返回NULL,错误代码存于errno.

范例
```c
#include <unistd.h>
main()
{
    char buf[80];
    getcwd(buf, sizeof(buf));
    printf("current working directory : %s\n", buf);
}
```

执行：
*current working directory :/tmp *
