# C Train For 18

## 大一C语言培训 (下)

+ [`结构体`](#结构体)
  + [`初认识结构体`](#初识结构体)
  + [`结构体的使用`](#结构体的使用)
    + [`结构体与数组`](#结构体与数组)
    + [`结构体与指针`](#结构体与指针)
  + [`结构体的应用--链表`](#结构体的应用)
    + [`结构体变量指针`](#结构体变量指针)
    + [`数组与链表`](#数组与链表)
    + [`链表概述`](#链表概述)
    + [`链表操作`](#链表操作)
+ [`字符串`](#字符串)
  + [`初识字符串`](#初识字符串)
  + [`字符串的输入与输出`](#字符串的输入与输出)
    + [`输出`](#输出)
    + [`输入`](#输入)
  + [`指向字符串的指针`](#指向字符串的指针)
  + [`常见的字符串操作`](#常见的字符串操作)
    + [`赋值`](#赋值)
    + [`加法`](#加法)
    + [`修改`](#修改)
    + [`比较`](#比较)
+ [`文件`](#文件)
  + [`为什么要有文件操作`](#为什么要有文件操作)
  + [`文件概述`](#文件概述)
  + [`文件的打开和关闭`](#文件的打开和关闭)
  + [`文件的读写操作`](#文件的读写操作)
    + [`写入数据`](#写入数据)
    + [`读取数据`](#读取数据)

## 结构体

### 初识结构体

为什么要有结构体？

在程序中，经常会遇到特定类型的实物需要使用过很多不同类型的数据来表述，如果全部都用单独的变量来指代每一个数据，就要定义很多非常繁琐的变量

```c
#include "stdio.h"
int main()
{
    char name[20] = "baoqianyue";
    int height = 175;
    int weight = 70;
    char sex = 'm';
    short age = 19;
    long  wealth  = 300000;
    printf("鲍骞月的个人信息：\n");
    printf("姓名：%s,身高：%d,性别：%c,年龄：%d,财产：%d\n",name,height,sex,age,wealth);
    return 0;
}
```

数组允许定义可存储相同类型数据项的变量，结构是 C 编程中另一种用户自定义的可用的数据类型，它允许您存储不同类型的数据项,而结构体的出现就很好的解决了这个问题

+ 结构体的构造

```c
struct 结构类型名
{
    数据类型1 成员变量1;
    数据类型2 成员变量2;
    数据类型3 成员变量3;
    ....
}; //<- 需要注意的地方
```

+ 定义结构体变量

```c
struct Stu{
    int height;//身高
    int weight;//体重
    char sex;//性别
    int age; //年龄
    long wealth;
};
```

这个结构体名是Stut，它内部有五个成员，分别为身高，体重，性别，年龄。定义形式与普通变量定义的方式一样，只不过不能立即初始化。

+ 结构体变量的初始化

结构体也是一种数据类型，在某种意义上与int，char这些基本数据类型是同级的，所以定义变量的方式是一样的。

```c
struct student stu1,stu2;
```

对于结构体，初始化并赋值的一般形式为

```c
strcut 结构类型名 结构变量 = {数据1，数据2,...};
```

+ 结构体成员的读取和赋值

结构体成员的获取形式为：

```c
结构体变量名.成员名;
```

为单个结构体变量赋值,定义结构体变量并赋值，在这里我们定义了一个名stu1的结构体变量，并且为这个结构体

```c
    Stu stu1;
    stu1.age = 19;
    stu1.height = 175;
    stu1.sex = 'm';
    stu1.wealth = 30000;
    stu1.weight = 70;
    printf("身高：%d,性别：%c,年龄：%d,财产：%d\n",stu1.height,stu1.sex,stu1.age,stu1.wealth);
```

+ 使用typedef简化变量名

```c
typedef StudentInfo Stu;
typedef int integer;
```

### 结构体的使用
  
#### 结构体与数组
  
+ 数组作为结构体的变量

举个例子：一家店雇佣了三个兼职人员，只需要他们在一个星期内来4天就可以，此时如何定义结构体？

```c
struct schedule{
    char name;
    char sex;
    int week1;
    int week2;
    int week3;
    int week4;
};
```

使用数组节省没必要的变量

```c
struct schedule{
    char name;
    char sex;
    int week[4];
};
//简化结构体的名字
typedef schedule S;
```

```c
int main()
{
    S sd1 = {'A','m',1,2,4,6};
    S sd2 = {'A','m',3,5,6,7};
    S sd3 = {'A','m',2,3,5,7};
    printf("姓名：%c,性别：%c,工作日：%d %d %d %d",
        sd1.name,sd1.sex,sd1.week[0],sd1.week[1],sd1.week[2],sd1.week[3]);
    return 0;
}
```

+ 保存结构的数组

```c
int main()
{
    S st[3] = { {'A','m',1,2,4,6 } , {'A','m',3,5,6,7} , {'A','m',2,3,5,7} };
    for(int i = 0;i < 3;i ++)
    printf("姓名：%c,性别：%c,工作日：%d %d %d %d\n",
        st[i].name,st[i].sex,st[i].week[i],st[i].week[1],st[i].week[2],st[i].week[3]);
    return 0;
}
```

#### 结构体与指针
  
+ 结构体与函数

结构体作为函数参数，传入函数进行赋值，并将赋值完的结构体返回给主函数
传参方式与其他类型的变量或指针类似

```c
#include "stdio.h"
struct complex_num{
    int real;
    int image;
};
typedef complex_num comp;

comp assign(comp num)
{
    puts("输入复数的实部：");
    scanf("%d",&(num.real));
    puts("输入复数的实部：");
    scanf("%d",&(num.image));
    return num;
}

int main()
{
    comp com1;
    com1 = assign(com1);
    printf("%d + %di",com1.real,com1.image);
    return 0;
}
```


### 结构体的应用

结构体的应用 -- 链表

#### 结构体变量指针

![train.png](https://upload-images.jianshu.io/upload_images/9140378-105df5dddce29c6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640)

+ 结构体变量成员指向自身

即将定义的结构体变量的地址赋予给所定义的结构体，这样定义的该结构体的指针域就只想了结构体本身

```c
struct table{
    int i;
    char c;
    struct table *st;
};
int main()
{
    table st1 = {1,'a'};
    st1.st = &st1;
    //使用结构体变量输出自身的2个成员的值
    printf("%d %c\n",st1.i,st1.c);
    //使用结构体指针域所指向的结构体输出数值
    printf("%d %c\n",st1.st->i,st1.st->c);
    return 0;
}
/*
1 a
1 a
*/
```

+ 结构体变量成员指向其他结构变量

即将定义的两个结构体变量，比方说定义了 st1 和 st2两个结构体变量，只需要将st2 的地址 赋给 st1 的指针域，这样 st1 的指针就指向了 st2

![train1.png](https://upload-images.jianshu.io/upload_images/9140378-b8b80be836c3d74a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/440)

```c
int main()
{
    table st1 = {1,'a'};
    table st2 = {2,'b'};
    st1.st = &st2;

    //使用结构体变量输出st1自身的2个成员的值
    printf("%d %c\n",st1.i,st1.c);

    //使用结构体指针域所指向的结构体输出数值,即 st2 中的数值
    printf("%d %c\n",st1.st->i,st1.st->c);

    //使用结构体变量输出st2自身的2个成员的值
    printf("%d %c\n",st2.i,st2.c);

    return 0;
}
/*
1 a
2 b
2 b
*/
```

#### 数组与链表

数组是由同类型的多个数据组成的，链表是由是由多个相同结构连接而成
但是数组中就可以存放结构体，为啥还要单独专门独立出来一个链表呢？这是因为数组的长度总是固定的，没办法动态的储存数据

```c
#include "stdio.h"
struct table{
    int i;
    char c;
    struct table *st;
};
int main()
{
    table tal[3] = {
        {1,'a'},
        {2,'b'},
        {3,'c'}
    };
    return 0;
}
```

上面的代码也能做到和链表一样的效果，甚至比链表还要简洁，但是如果程序中的结构数目是用户自己决定的话，或者说结构体的数目是位未知的，那怎么办?数组的长度可以在程序运行时不能被更改，所以说，数组跟结构体搭配的前提时数组的长度固定并且已知

#### 链表概述

![timg.jpg](https://upload-images.jianshu.io/upload_images/9140378-c16769111551b03e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/840)

链表的最小单元 -- 结点

```c
struct table
{
    int i;
    char c;
    struct table *next;
}
strcut table st1 = {1,'a'};
struct table st2 = {2,'b'};
st1.next = &st2;
```

链表的组成部分

一个连边通常由3部分组成：投机欸但、数据结点和尾结点

+ 头节点：数据域的变量不指代数据，指针域的指针变量指向链表的第一个数据结点。通常情况下，使用‘ 单链表 ’ 的程序只记录头节点，其他节点都是通过头节点一次获取得到的
+ 数据结点：数据域的变量指代实实在在的数据，指针域的指针变量用于指向下一个数据结点
+ 尾结点：数据域的变量指代实实在在的数据，指针域的指针变量被赋予为空，便是没有指向任何地方

#### 动态创建链表

构建3步骤

+ 构造一个结构类型，此结构类型必须包含至少一个成员指针，此指针要指向此结构类型，
+ 定义3个结构体类型的指针，按照用途可以命名为，p_head,p_rail,p_new
+ 动态生成新的结点，为各成员变量赋值，最后加到链表当中

构造专用于链表的结构

```c
struct node
{
    short i;   数据域
    char c;   ///数据域
    struct node *next;  //指针域，用于指向下一个结点
}
```

定义结构体指针

```c
struct node *p_head,*p_rail,*p_new ;
```

使用malloc() 动态申请储存空间作为新节点，声明形式：

```c
void malloc(unsigned int num_bytes);
```

+ num_bytes声明的空间大小
+ 返回void类型的指针
+ 要注意的是，生命完要把空间释放掉，盗用的函数为 free();

![train20.png](https://upload-images.jianshu.io/upload_images/9140378-e9fbe3bdd6db4850.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)

接下来写一个动态创建链表的实例：

首先构造结构体

```c
struct node {
    short i;
    char c;
    struct node *next;
};
```

构造一个含有3个结点的链表

```c
struct node node1 = {1,'A'};
struct node node2 = {2,'B'};
struct node node3 = {3,'C'};
node1.next = &node2;
node2.next = &node3;
```

<div align="center">

![train21.png](https://upload-images.jianshu.io/upload_images/9140378-32814c518bae8414.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)

</div>

遍历链表输出数据

```c
struct node *p;
p = &node1;
for(int j = 0;j < 3;j ++)
{
    printf("node:%d %c",p->i,p->c);
    p = p->next;
}
```

动态生成新节点

```c
struct node *p_new;
p_new = (struct node *)malloc(sizeof(struct node));
p_new->i = 4;
p_new->c = 'd';
```

添加到链表当中

```c
node3.next = p_new;
```

#### 链表操作

插入结点到链表

+ 插入结点到第一个数据结点前

<div align="center">

![train22.png](https://upload-images.jianshu.io/upload_images/9140378-884b75f47c63a109.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/740)

</div>

```c
struct node p_new = (struct node *)malloc(sizeof(struct node));  //创建新结点，并为其开辟空间
scanf("%d%c",&(p_new->i),&(p_new->c));  //录入结点数据
//插入节点
p_new->next = p_head-next;
p_head->next = p_new;
```

+ 插入结点到链表中间

<div align="center">

![train23.png](https://upload-images.jianshu.io/upload_images/9140378-320eb2dd5952ee8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/740)

</div>

```c
struct node p_new = (struct node *)malloc(sizeof(struct node));  //创建新结点，并为其开辟空间
p_new->i = 2;
p_new->c = 'B';

struct node *p_front = p_head->next;
p_new->next = p_front->next;
p_front->next = p_new;
```

+ 插入节点到链表末尾

![train24.png](https://upload-images.jianshu.io/upload_images/9140378-b772cb8890126923.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```c
while(1)
{
    if(p-next == NULL)
    {
        p_rail = p;
        break;
    }
    p = p->next;
}
p_rail->next = p_new;
p_tail = p_new;
```

+ 删除链表中的结点

<div align="center">

![train25.png](https://upload-images.jianshu.io/upload_images/9140378-3582fbb7b736eb09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/440)

</div>

```c
void del_list(struct node *p_head,int pos)
{
    strct node *p_front,*p_del;
    p_front = p_head;
    for(int i = 0;i <= pos - 1;i ++)
    {
        p_front = p_front->next;
    }
    p_del = p_front->next;
    p_front->next = p_del->next;
    free(p_del);
}
```

## 字符串

### 初识字符串

什么是字符串？

在 C 语言中，字符串实际上是使用 null 字符 '\0' 终止的一维字符数组。因此，一个以 null 结尾的字符串，包含了组成字符串的字符。

下面的声明和初始化创建了一个 "Hello" 字符串。由于在数组的末尾存储了空字符，所以字符数组的大小比单词 "Hello" 的字符数多一个

```c
char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
```

依据数组初始化规则，您可以把上面的语句写成以下语句：

```c
char greeting[] = "Hello";
```

定义的字符串的内存表示：

![train17.jpg](https://upload-images.jianshu.io/upload_images/9140378-c4e53c41e194053a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)

### 字符串的输入与输出

#### 输出

普通方式

```c
char arr[] = "Hello!";
int i = 0;
while(arr[i] != '\0')
{
    printf("%c",arr[i]);
    i ++;
}
```

特殊方式

```c
char arr[] = "Hello!";
printf("%s\n",arr);
```

其他方式,函数 putchar();

```c
char arr[] = "Hello!";
int i = 0;
while(arr[i] != '\0')
{
    putchar(arr[i]);
    i ++;
}
```

#### 输入

普通方式

```c
int i = -1;
do
{
    i ++;
    scanf("%c",&arr[i]);
}while(arr[i] != '\n');
arr[i] = '\0';
```

特殊方式

```c
scanf("%s",arr);
```

其他方式

```c
arr[i] = getchar();
```

### 指向字符串的指针

```c
char arr[10] = {0};
char *p = arr;
int i = -1;
do
{
    i ++;
    scanf("%d",p + i);
}while(*(p + i) != '\n');
*(p + i) = '\0';
i = 0;
while(*(p + i) != '\0')
{
    printf("%c",*(p + i));
    i ++;
}
```

gets 和 puts()

```c
cahr arr[20] = {0};
char *p = arr;
gets(p);
puts(p);
```

### 常见的字符串操作

+ strcpy(p, p1) 复制字符串
+ strncpy(p, p1, n) 复制指定长度字符串
+ strcat(p, p1) 附加字符串
+ strncat(p, p1, n) 附加指定长度字符串
+ strlen(p) 取字符串长度
+ strcmp(p, p1) 比较字符串
+ strcasecmp忽略大小写比较字符串
+ strncmp(p, p1, n) 比较指定长度字符串
+ strchr(p, c) 在字符串中查找指定字符
+ strrchr(p, c) 在字符串中反向查找
+ strstr(p, p1) 查找字符串

#### 赋值

何为赋值？

```c
float f1 = 3.654;
float f2;
f2 = f1;
```

字符串拷贝函数

将src指向的字符串拷贝到des指向的字符串数组中去，结束符也一同进行拷贝，size参数也可以拷贝制定长度的字符串，建议des为字符数组

```c
char *strcpy(char*des,char*src);
char *strncpy(char *des,char *src,int size);
```

#### 加法

字符串的连接函数

将str2指向的字符串连接到str1指向的字符后面，同时会删除str1后面的’\0’,返回的是str1指向字符串的首地址重点内容

```c
char * strcat(const *char str1,const *char str2);
char *strncat(const *char str1,const *char str2,int size);
```

错误的加法运算

```c
char *p1 = "super";
char *p2 = "market";
char *p3 = p1 + p2;  //错误的加法
```

```c
char arr[30] = {0};
char *p3 = arr;
p3 = strcat(p3,p1);
p3 = strcat(p3,p2);
p3 = strncat(p3,p1,1);  //将p1所指向的字符的第一个字符加到p3所指字符串的末尾
// p3 = supermarkets
```

#### 修改

```c
cahr arr[] = "Nes!";
cahr *p = arr;
*p = 'Y';
```

指针p指向了arr字符串的字符串，借助 p 可以任意修改字符串中的任意字符,但是借助指针修改一个字符还比较容易，批量的话就需要 库函数 strset

```c
char *strset(char *s,char c);  //将字符串s中的字符全部设成字符 c
char *strnset(char *s,char c,int n); //将s指向的字符串的前n个字符都设成c
```

```c
char p1[] = "Are you ok";
strset(p1,'a');
// p1 aaaaaaaaaa

strset(p2,'b',2);

//p1 bbaaaaaaaa
```

#### 比较

字符串比较函数

错误的比较方式

```c
char arr[] = "What";
char arr2[] = "That";
arr1 = arr2;  //错误的比较方式
```

按照ascii码来进行比较，并由函数返回值进行判断
返回0,字符串1等于字符串2,
大于0,字符串1大于字符串2,
小于0,字符串1小于字符串2,

```c
int strcmp(const char *str1,const char *str2);
int strncmp(const char *str1,const char *str2,int size);

char buf1[] = "aaa";
char buf2 = "bbb";
int ptr = strcmp(buf2,buf1);//ptr < 0
```

## 文件

<div align="center">

![train19.gif](https://upload-images.jianshu.io/upload_images/9140378-68d9870e8f7a4e41.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/440)

</div>

### 为什么要有文件操作

两个没有解决的问题

不得不再次运行程序

+ 我们运行计算机上的程序，然后不断输入数据给程序，然后得到程序对程序的处理结果，如果关掉程序的话，再想看到那些数据，就不得不再次运行程序。而且如果数据量过大的话，没办法留住这些数据

不得不重新输入数据

+ 每次在循行程序的时候吗，每运行一次都要重新的从简盘再次录入数据，而文件的运用帮我们解决了这个繁琐的问题

### 文件概述

+ 定位文件
+ 读取、写入文件
+ 关闭文件

<div align="center">

![train16.png](https://upload-images.jianshu.io/upload_images/9140378-3e2ccfe2d81cd5ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)

</div>

### 文件的打开和关闭

#### 打开文件

可以使用 fopen( ) 函数来创建一个新的文件或者打开一个已有的文件，这个调用会初始化类型 FILE 的一个对象，类型 FILE 包含了所有用来控制流的必要的信息。下面是这个函数调用的原型：

```c
FILE *fopen( const char * filename, const char * mode );
```

在这上面的函数原型里面，filename 是字符串，用来命名文件，访问模式 mode 的值可以是下列值中的一个：
|||
|:--|:--|
|模式|描述|
|r|打开一个已有的文本文件，允许读取文件。|
|w|打开一个文本文件，允许写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会从文件的开头写入内容。如果文件存在，则该会被截断为零长度，重新写入。|
|a|打开一个文本文件，以追加模式写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会在已有的文件内容中追加内容。|
|r+|打开一个文本文件，允许读写文件。|
|w+|打开一个文本文件，允许读写文件。如果文件已存在，则文件会被截断为零长度，如果文件不存在，则会创建一个新文件。|
|a+|打开一个文本文件，允许读写文件。如果文件不存在，则会创建一个新文件。读取会从文件的开头开始，写入则只能是追加模式。|

如果处理的是二进制文件，则需使用下面的访问模式来取代上面的访问模式：

```c
"rb", "wb", "ab", "rb+", "r+b", "wb+", "w+b", "ab+", "a+b"
```

#### 关闭文件

为了关闭文件，请使用 fclose( ) 函数。函数的原型如下：

```c
int fclose( FILE *fp );
```

如果成功的关闭了文件，会清空缓冲区中的数据，并释放用于该文件的所有内存，这个函数就会返回0，如果关闭文件的时候放生错误，函数会返回EOF，而EFO是一个常量

```c
#include "stdio.h"
#include "stdlib.h"
int main()
{
    //定义一个指向文件类型的指针
    FILE *fp;
    //打开一个已有的文件
    fp = fopen(text.txt,"r"); //错误的方式
    fp = fopen("D:\Desktop\text.txt","r");  //错误的方式
    if(fp == NULL )
    {
        printf("打开文件失败！\n");
        exit(0);
    }
    printf("打开文件成功！\n");
    fclose(fp);


    //如果想要在字符串中使用 '\' 必须要写成 '\\' 来转义
    fp = fopen("D:\\Desktop\\text.txt","r");
    //printf("D:\\Desktop\\text.txt\n");
    //printf("D:\Desktop\text.txt\n");

//使用访问模式 'w' 打开文件，会发现文件之前保存的内容被清零
    fp = fopen("D:\\Desktop\\text.txt","w");

    return 0;
}
```

### 文件的读写操作

#### 写入数据

想要让程序在文件中写入文件，在程序与文件建立关联的时候，必须保证打开方是可写的，有 4 中方式可以将数据写入文件当中

字符方式  

程序可以以字符为单位，一个字符一个字符的将数据写入到文件当中，需要的函数是 fputc(),声明如下：

```c
int fputc(char c,FILE *stream);
```

+ 参数 c 指代的是一个将要被写入文件的字符
+ 参数 stream 是一个文件指针，指向将要被写入的文件
+ 写入成功，返回 写入字符的ASCII 码值

```c
char ch; //定义一个字符串
int i = 0;
ch = getchar();
while(ch != '\n');
{
    i = fputc(ch,fp);  // 以字符为单位，写入到text.txt文件
    if(i == -1)
    {
        puts("字符写入失败！");
        exit(0);
    }
    ch = getchar();
}
```

格式化方式

如果写入文件的内容有特定的格式要求，可以使用格式化的方式将数据写入到文本

stdio.h 提供了一个库函数 fprintf(),可以达到这个目的，声明如下：

```c
int fprintf(FILE *stream,const char *format[, argument ] ...);
```

和 printf()的使用方法一致

+ 参数的 stream 是指向将要被写入数据的文件的文件指针
+ 参数 format 是格式化的字符串
+ 参数 argument 是可选的，如果 format 中有格式符，argument 就是对应的变量
+ 函数 fprintf() 返回实际输入到文件中的字符的个数

```c
struct info
{
    short no;
    char name[10];
    char sex[6];
};

struct info info_st[3] ={
    {1,"baoqianyue","men"},
    {2,"lihao","men"},
    {3,"wanghao","men"}
};
for(int i = 0;i < 3;i ++)
{
    fprintf(fp,"No = %d\tname = %-8s\tsex = %-6s\n",info_st[i].no,
        info_st[i].name,info_st[i].sex);
}
```

字符串方式

对于程序中的字符串，除了以字符串为单位，一个字符一个字符的录入之外，还可以以字符串为单位，一次性的写入一串字符，需要用到的路函数是 fputs(),它的声明是：

```c
int puts(const char *str,FILE *stream);
```

+ str 写入文件的字符串指针
+ stream 文件指针
+ 字符串写入失败的话，同样也是会返回 -1

```c
char c[100];
gets(c);
int value = fputs(c,fp);
if(value == -1)
{
    puts("字符串写入失败！\n");
    exit(0);
}
```

二进制方式

储存为文件的数据形式一般为两种，分别是字符形式 和 二进制 形式，使用二进制方式向文件写入数据，需要用到的库函数是 fwrite(),它的声明是：

```c
int fwrite(const void *buffer,int size,int count,FILE *stream);
```

+ buffer 无类型指针
+ size 是要被写入到文件的数据的大小
+ count 是size 为单元的单元的个数
+ stream 文件指针

```c
struct info
{
    short no;
    char name[10];
    char sex[6];
};

struct info info_st[3] ={
    {1,"baoqianyue","men"},
    {2,"lihao","men"},
    {3,"wanghao","men"}
};

int count = fwrite(info_st,sizeof(struct info),3,fp);  //写入数据到文件

```

#### 读取数据

字符方式

以字符为单位，一个一个从文本文件读取数据，使用的库函数为 fgetc()，声明方式如下:

```c
int fgetc(FILE *stream);
```

+ 参数 stream 是一个文件指针，指向将要被写入的文件
+ 写入成功，返回 写入字符的ASCII 码值，写入失败，返回 -1

```c
char ch = fgetc(fp);
while(ch != -1)
{
    putchar(ch);
    ch = fgetc(fp);
}
```

格式化方式

要格式化的一次性从一个文件读取多个字符，用到的库函数是fscanf(),声明方式如下：

```c
int fscanf(FILE *stream,const char *format[, argument ]...);
```

+ 参数的 stream 是指向将要被写入数据的文件的文件指针
+ 参数 format 是格式化的字符串
+ 参数 argument 是可选的，如果 format 中有格式符，argument 就是对应的变量
+ 函数 fprintf() 返回读取到的文件中的字符的个数

```c
struct info
{
    short no;
    char name[10];
    char sex[6];
};

struct info info_st[3] ={
    {1,"baoqianyue","men"},
    {2,"lihao","men"},
    {3,"wanghao","men"}
};
for(int i = 0;i < 3;i ++)
{
    fscanf(fp,"No = %d\tname = %-8s\tsex = %-6s\n",&info_st[i].no,
       &info_st[i].name,&info_st[i].sex);
}
```

字符串方式

一次性全部取出字符串，用到的库函数为 fgets(),它的声明如下：

```c
char *fgets(char *str,int n,FILE *stream);
```

+ str 写入文件的字符串指针
+ n 是读取字符串结束标志 "\0"
+ stream 文件指针
+ 字符串读取成功的话，返回指向字符串的指针，否则返回NULL

```c
char arr[15] = {0};
char *p = fgets(arr,15,fp);
while(p != NULL)
{
    printf("%s",arr);
    p = fgets(arr,15,fp);
}
```

二进制方式

以二进制写入文件通常是给程序自己看的，就是俗称的乱码，用到的库函数是fread(),它的声明方式是：

```c
int fr(eadconst void *buffer,int size,int count,FILE *stream);
```

```c
int count = fread(info_st,sizeof(struct info),3,fp);  //从文件读取数据
```