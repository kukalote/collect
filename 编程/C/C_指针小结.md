C_指针小结1

##什么是指针？

**指针**是一个*变量*，其值为另一个变量的地址，即，内存位置的直接地址。就像其他变量或常量一样，您必须在使用指针存储其他变量地址之前，对其进行声明。指针变量声明的一般形式为：

	type *var-name;
> 在这里，type 是指针的基类型，它必须是一个有效的 C 数据类型，var-name 是指针变量的名称。
	`int    *ip;    /* 一个整型的指针 */`
	所有指针的值的实际数据类型，不管是整型、浮点型、字符型，还是其他的数据类型，都是一样的，都是一个代表内存地址的长的十六进制数。不同数据类型的指针之间唯一的不同是，指针所指向的变量或常量的数据类型不同。

## 指针的使用

```C
#include <stdio.h>

int main ()
{
   int  var = 20;   /* 实际变量的声明 */
   int  *ip;        /* 指针变量的声明 */

   ip = &var;  /* 在指针变量中存储 var 的地址 */

   printf("Address of var variable: %x\n", &var  );

   /* 在指针变量中存储的地址 */
   printf("Address stored in ip variable: %x\n", ip );

   /* 使用指针访问值 */
   printf("Value of *ip variable: %d\n", *ip );

   return 0;
}
```

返回结果如下 :

```C
Address of var variable: bffd8b3c
Address stored in ip variable: bffd8b3c
Value of *ip variable: 20
```

## 空指针事项

在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个 **NULL** 值是一个良好的编程习惯。赋为 **NULL** 值的指针被称为**空指针**。

**NULL** 指针是一个定义在标准库中的值为**零**的常量。请看下面的程序：

```C
int  *ptr = NULL;
printf("ptr 的值是 %x\n", ptr  );

//ptr 的值是 0
```

>在大多数的操作系统上，程序不允许访问地址为 **0** 的内存，因为该内存是操作系统保留的。然而，内存地址 **0** 有特别重要的意义，它表明该指针不指向一个可访问的内存位置。但按照惯例，如果指针包含空值（零值），则假定它不指向任何东西。

```C
如需检查一个空指针，您可以使用 if 语句，如下所示：
if(ptr)     /* 如果 p 非空，则完成 */
if(!ptr)    /* 如果 p 为空，则完成 */
```

## 指针操作

#### 指针递减/加

程序中使用指针代替数组，因为变量指针可以递增，而数组不能递增，因为数组是一个**常量指针**。下面的程序递增变量指针，以便顺序访问数组中的每一个元素：

```C
#include <stdio.h>
const int MAX = 3;

int main () {
   int  var[] = {10, 100, 200};
   int  i, *ptr;

   /* 指针中的数组地址 */
   ptr = var;
   for ( i = 0; i < MAX; i++) {
	  printf("Address of var[%d] = %x\n", i, ptr );
	  printf("Value of var[%d] = %d\n", i, *ptr );
	  /* 移动到下一个位置 */
	  ptr++;
   }
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

	Address of var[0] = bf882b30
	Value of var[0] = 10
	Address of var[1] = bf882b34
	Value of var[1] = 100
	Address of var[2] = bf882b38
	Value of var[2] = 200

#### 指针比较

指针可以用关系运算符进行比较，如 `==`、`<` 和 `>`。如果 p1 和 p2 指向两个相关的变量，比如同一个数组中的不同元素，则可对 p1 和 p2 进行大小比较。

```C
#include <stdio.h>
const int MAX = 3;

int main () {
   int  var[] = {10, 100, 200};
   int  i, *ptr;

   /* 指针中第一个元素的地址 */
   ptr = var;
   i = 0;
   while ( ptr <= &var[MAX - 1] ) {
      printf("Address of var[%d] = %x\n", i, ptr );
      printf("Value of var[%d] = %d\n", i, *ptr );

      /* 指向上一个位置 */
      ptr++;
      i++;
   }
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

	Address of var[0] = bfdbcb20
	Value of var[0] = 10
	Address of var[1] = bfdbcb24
	Value of var[1] = 100
	Address of var[2] = bfdbcb28
	Value of var[2] = 200


