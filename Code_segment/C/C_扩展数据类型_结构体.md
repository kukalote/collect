C_结构体初识

## 结构体

**结构**是 C 编程中除数组外,另一种用户自定义的可用的数据类型，它允许您存储不同类型的数据项.

## 定义结构

为了定义结构，您必须使用 **struct** 语句。**struct** 语句定义了一个包含多个成员的新的数据类型，**struct** 语句的格式如下：

```c
struct [structure tag]
{
   member definition;
   member definition;
   ...
   member definition;
} [one or more structure variables];
```

**structure tag** 是可选的，每个 **member definition** 是标准的变量定义，比如 int i;  或者其他有效的变量定义。在结构定义的末尾，最后一个分号之前，您可以指定一个或多个结构变量，这是可选的。下面是声明 Book 结构的方式：
> struct Books {
	   char  title[50];
	   char  author[50];
	   char  subject[100];
	   int   book_id;
	} book, *pbook;

## 访问结构成员

为了访问结构的成员，我们使用成员访问运算符（`.`）。成员访问运算符是结构变量名称和我们要访问的结构成员之间的一个句号。您可以使用 **struct** 关键字来定义结构类型的变量。下面的实例演示了结构的用法：

```c
#include <stdio.h>
#include <string.h>

struct Books {
   char  title[50];
   int   book_id;
};

int main( ) {
   struct Books Book1;        /* 声明 Book1，类型为 Book */

   /* Book1 详述 */
   strcpy( Book1.title, "C Programming");
   Book1.book_id = 6495407;

   /* 输出 Book1 信息 */
   printf( "Book 1 title : %s\n", Book1.title);
   printf( "Book 1 book_id : %d\n", Book1.book_id);

   return 0;
}
```

输出结果如下 :

	Book 1 title : C Programming
	Book 1 book_id : 6495407

##结构作为函数参数

您可以把结构作为函数参数，传参方式与其他类型的变量或指针类似。您可以使用上面实例中的方式来访问结构变量：

```c
#include <stdio.h>
#include <string.h>

struct Books {
   char  title[50];
   int   book_id;
};

/* 函数声明 */
void printBook( struct Books book );
int main( ) {
   struct Books Book1;        /* 声明 Book1，类型为 Book */

   /* Book1 详述 */
   strcpy( Book1.title, "C Programming");
   Book1.book_id = 6495407;

   /* 输出 Book1 信息 */
   printBook( Book1 );
   return 0;
}
void printBook( struct Books book ) {
   printf( "Book title : %s\n", book.title);
   printf( "Book book_id : %d\n", book.book_id);
}
```

输出结果如下 :

	Book title : C Programming
	Book book_id : 6495407

##指向结构的指针

您可以定义指向结构的指针，方式与定义指向其他类型变量的指针相似，如下所示：

	struct Books *struct_pointer;	//定义
	struct_pointer = &Book1;		//赋值
	struct_pointer->title;			//使用指向该结构的指针访问结构的成员

让我们使用结构指针来重写上面的实例 :

```c
#include <stdio.h>
#include <string.h>

struct Books {
   char  title[50];
   int   book_id;
};

/* 函数声明 */
void printBook( struct Books *book );
int main( ) {
   struct Books Book1;        /* 声明 Book1，类型为 Book */

   /* Book1 详述 */
   strcpy( Book1.title, "C Programming");
   Book1.book_id = 6495407;

   /* 通过传 Book1 的地址来输出 Book1 信息 */
   printBook( &Book1 );

   printf( "Book title : %s\n", (&Book1)->title);
   printf( "Book book_id : %d\n", (&Book1)->book_id);
   return 0;
}
void printBook( struct Books *book ) 	{
   printf( "Book title : %s\n", book->title);
   printf( "Book book_id : %d\n", book->book_id);

}
```

输出结果如下 :

	Book title : C Programming
	Book book_id : 6495407

## 位域

有些信息在存储时，并不需要占用一个完整的字节，而只需占几个或一个二进制位。例如在存放一个开关量时，只有 0 和 1 两种状态，用 1 位二进位即可。为了节省存储空间，并使处理简便，C 语言又提供了一种数据结构，称为"**位域**"或"**位段**"。

所谓"**位域**"是把一个字节中的二进位划分为几个不同的区域，并说明每个区域的位数。每个域有一个域名，允许在程序中按域名进行操作。这样就可以把几个不同的对象用一个字节的二进制位域来表示。

位域定义与结构定义相仿，其形式为：

    struct 位域结构名
        { 类型说明符 位域名: 位域长度;... };

例如：

>struct bs{
		int a:8;
		int b:2;
		int c:6;
	};

对于位域的定义尚有以下几点说明：

- 一个位域必须存储在同一个字节中，不能跨两个字节。如一个字节所剩空间不够存放另一位域时，应从下一单元起存放该位域。也可以有意使某位域从下一单元开始。
- 由于位域不允许跨两个字节，因此位域的长度不能大于一个字节的长度，也就是说不能超过8位二进位。如果最大长度大于计算机的整数字长，一些编译器可能会允许域的内存重叠，另外一些编译器可能会把大于一个域的部分存储在下一个字中。
- 位域可以是无名位域，这时它只用来作填充或调整位置。无名的位域是不能使用的。

##位域的使用

位域的使用和结构成员的使用相同，其一般形式为：

    位域变量名·位域名

位域允许用各种格式输出。

请看下面的实例：

```c
main(){
    struct bs{
        unsigned a:1;
        unsigned b:3;
        unsigned c:4;
    } bit,*pbit;

    bit.a=1;	/* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
    bit.b=7;	/* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
    bit.c=15;	/* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
    printf("%d,%d,%d\n",bit.a,bit.b,bit.c);	/* 以整型量格式输出三个域的内容 */

    pbit=&bit;	/* 把位域变量 bit 的地址送给指针变量 pbit */
    pbit->a=0;	/* 用指针方式给位域 a 重新赋值，赋为 0 */
    pbit->b&=3;	/* 使用了复合的位运算符 "&="，相当于：pbit->b=pbit->b&3，位域 b 中原有值为 7，与 3 作按位与运算的结果为 3（111&011=011，十进制值为 3） */
    pbit->c|=1;	/* 使用了复合位运算符"|="，相当于：pbit->c=pbit->c|1，其结果为 15 */
    printf("%d,%d,%d\n",pbit->a,pbit->b,pbit->c);	/* 用指针方式输出了这三个域的值 */
}
```