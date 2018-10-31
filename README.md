# 2018-C语言培训
> 本次培训采用C99标准
> 本次培训采用C99标准
# Hello World
- ####  你好，世界！
``` C
#include<stdio.h>  // 包含另一个文件

/* 这是一个简单的演示程序 */
int main()   // 函数名
{ // 函数体开始
	int num; // 声明
	num = 1; // 赋值表达式语句
	printf("Hello World\n"); // 调用函数
	return 0; // return 语句
} // 结束
```
- #### 程序细节
  - `#include`：这行代码是C语言预编译指令
  - `int main()函数`：C语言程序一定是从`main()`函数开始执行，并且一个项目只能有一个`main`函数。
      - `void main()`：注意，一些编译器允许这样写，但是所有的标准都未认可这种写法。而`int main()`是标准写法，使用标准写法，在将程序从一个编译器切换到另一个编译器时一般不会出现什么问题。
  - `/* ... */`：这是C语言中的注释，它允许同时注释多行。对于单行注释也可使用`//`。
  - `{ ... }`：这是花括号，一般而言，所有C函数都要使用花括号标记函数体的开始和结束。
  - `int num`：这是声明，声明一个变量一般形式是`关键字 标识符`，例如`char str`。
  - `num = 1`：赋值表达式语句。
  - `printf()`：这是C语言的一个标准输出函数。括号中的`Hello World\n`是这个函数的`实际参数`。
  - `return 0`：C语言标准中，要求main()函数返回0。但如果省略不写这一句呢？程序在运行至最外面的右花括号时会返回0。因此，可以省略main()函数的`return`语句，但是不要在其他有返回值的函数中漏掉它。强烈建议读者养成在main()函数中保留`return`语句的好习惯。

- #### 代码编写规范
  - 标识符命名规范：标识符只能由字母、数字和下划线组成，并且第一个字符必须是字母或下划线
  - 变量名应该有具体含义
  - 每条语句占一行
  - 对齐缩进4个空格字符
  - ...


# 数据类型
- #### C数据类型
![type](https://github.com/android-nuc/17-C-Train/raw/master/image/c_language.png)

- #### 什么是位、字节和字
> 位、字节和字是描述计算机数据单元或存储单元的术语。<br>
> 最小的存储单元是位(bit)，可以储存0或1.虽然1位储存的信息有限，但是计算机中位的数量十分庞大。位是计算机内存的基本构建块。<br>
> 字节(byte)是常用的计算机存储单位。对于几乎所有的机器，1字节均为8位。这是字节的标准定义，至少在衡量存储单位时是这样。既然1位可以表示0或1，那么8位字节就有256（2的8次方）种可能的0、1组合。通过二进制编码（仅用0和1便可表示数字），便可表示0~255的整数或一组字符。<br>
> 字(word)是设计计算机时给定的自然存储单位，对于8位的微型计算机（如，最初的苹果机），1个字长只有8位。从那以后，个人计算机字长增至16位、32位，直到目前的64位，计算机的字长越大，其数据转移越快，允许的内存访问也快的多。<br>
> ————《C Primer Plus》

`注意：C语言把1字节定义为char类型占用的位(bit)数。通常，char类型被定义为8位的存储单元。`

- #### 整数类型
  
|类型|存储大小|值范围|
|-|-|-|
|`char`|1 byte|`-128 到 127 或 0 到 255`|
|unsigned char|1 byte|0 到 255|
|signed char|1 byte|-128 到 127|
|int|2 或 4 bytes|-32,768 到 32,767 或 -2,147,483,648 到 2,147,483,647|
|unsigned int|2 或 4 bytes|0 到 65,535 或 0 到 4,294,967,295|
|short|2 bytes|-32,768 到 32,767|
|unsigned short|2 bytes|0 到 65,535|
|long|4 bytes|-2,147,483,648 到 2,147,483,647|
|unsigned long|4 bytes|0 到 4,294,967,295|

不同平台上数据类型的取值范围有所差异，为了得到某个类型或某个变量在特定平台上的准确大小，可以使用`sizeof`运算符，得到对象或类型的存储字节大小。

``` C
#include<stdio.h>

int main()
{
	printf("Storage size for int : %d \n", sizeof(int));
	return 0;
}
```
输出：
```
Storage size for int : 4
```
按照1个字节8位计算，那4个字节能够存储$2^{4*8}$，即$2^{32} = 4294967296$，正负数各分一半，也就是-2,147,483,648 到 2,147,483,647。



- #### 浮点类型

类型|存储大小|值范围|精度
|-|-|-|-|
float|4 byte|1.2E-38 到 3.4E+38|6 位小数
double|8 byte|2.3E-308 到 1.7E+308|15 位小数
long double|10 byte|3.4E-4932 到 1.1E+4932|19 位小数

- #### 基本数据类型所占字节数与三个方面因素有关
  - CPU位宽（即你的CPU是多少为的）
  - 操作系统位宽（笼统说就是操作系统位数，操作系统位宽取决于CPU位宽）
  - 编译器类型和版本

- #### 整型溢出


- EX.1<br>
计算机中的计算只有二进制加法。因此，计算机在计算时实际上是取它们的补码进行加法运算。
``` C
unsigned int a;
int b = -1;
a = b;
printf("a=%u",a);
```
输出：
```
a=4294967295
```
让我们来分析一下，<br>
首先int型的-1，对应二进制<br>
取原码：`1000000000000000000000000000001`<br>
取反码：`1111111111111111111111111111110`<br>
取补码：`1111111111111111111111111111111`<br>
而`unsigned int`型最大值对应的补码也是它，因此在赋值给`a`后，就得到了它的最大值。

- EX.2<br>
如果整数超出了相应类型的取值范围会怎样？
``` C
#include<stdio.h>

int main()
{
	int i = 2147483647;
	unsigned int j = 4294967295;
	printf("%d %d %d\n", i, i+1, i+2);
	printf("%u %u %u\n", j, j+1, j+2);

	return 0;
}
```
输出：
```
2147483647 -2147483648 -2147483647
4294967295 0 1
```
从输出结果不难发现，当达到它们能表示的最大值时，会重新从起点开始。只不过`unsigned int`最小值为0，而`int`型最小值为-2147483648。

[关于原码、反码和补码更多介绍](https://www.cnblogs.com/zhangziqiu/archive/2011/03/30/ComputerCode.html)

# 明示常量：#define
编译时，在预处理阶段，预处理器会查找一行中以`#`号开始的预处理指令。预处理指令从`#`号开始执行，到后面的第一个换行符为止。也就是说，指令的长度仅限于一行（逻辑行）。

- **类对象宏**
![obj](img/1.jpg)

宏的名称同样需要遵守C变量的命名规则：只能使用字符、数字和下划线，且首字符不能是数字。<br>
可以把它看做是一种记号，程序在编译时会把记号替换为它对应的值。

例子：
``` C
#include<stdio.h>
#define WORD "Hello World!"
#define OP 6+6
#define LINE 10

int main()
{
	char str[15] = WORD;
	int sum = OP;

	for(int i = 0; i < LINE; i++)
	{
		printf("%d\t%s\t%d\n", i, str, sum);
	}

	return 0;
}
```

- **类函数宏**<br>
类函数宏可以像函数那样传入参数，它的组成如下

![2](img/2.jpg)

例子：
``` C
#include<stdio.h>
#define MEAN(X,Y) ((X)+(Y))/2

int main()
{
	int mean = MEAN(10, 20);
	printf("%d", mean);

	return 0;
}
```
- 注意：`预处理器不做计算、不求值、只替换字符序列`！！

``` C
#include<stdio.h>
#define SQUARE(X) X*X

int main()
{
	int x = 5;
	printf("%d\n", SQUARE(x));
	printf("%d\n", SQUARE(x+2));
	printf("%d\n", 100/SQUARE(2));

	return 0;
}
```
预处理器仅仅是在编译时替换了字符序列，所以在写替换体时，一定要注意参数的作用范围。此处出现的问题可以通过添加括号解决：`(X)*(X)`。

- **常用的宏**
``` C
#define MAX(X,Y) ((X) > (Y) ? (X) : (Y))
#define ABS(X) ((X) < 0 ? -(X) : (X))
#define ISSIGN(X) ((X) == '+' || (X) == '-' ? 1 : 0)
```
再次强调，宏只是在程序编译时，替换掉记号位置的字符序列。

# 常用运算符
- 常用运算符

赋值|自增自减|算术|逻辑|比较|成员访问|其他
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
a = b<br>a += b<br>a -= b<br>a *= b<br>a /= b<br>a %= b<br>a &= b<br>a &#124;= b<br>a ^= b<br>a <<= b<br>a >>= b|++a<br>--a<br>a++<br>a--<br>|+a<br>-a<br>a + b<br>a - b<br>a * b<br>a / b<br>a % b<br>~a<br>a & b<br>a &#124; b<br>a ^ b<br>a << b<br>a >> b<br>|!a<br>a && b<br>a &#124;&#124; b<br>|a == b<br>a != b<br>a < b<br>a > b<br>a <= b<br>a >= b<br>|a[b]<br>*a<br>&a<br>a->b<br>a.b<br>|a(...)<br>a, b<br>(type) a<br>? :<br>sizeof<br>_Alignof (C11 起)

[关于C运算符优先级](https://zh.cppreference.com/w/c/language/operator_precedence)
- 位运算符

|运算符|含义|描述|
|-|-|-|
|&|按位与|如果两个相应的二进制位都为1，则该位的结果值为1，否则为0|
|&#124;|按位或|两个相应的二进制位中只要有一个为1，该位的结果值为1|
|^|按位异或|若参加运算的两个二进制位值相同则为0，否则为1|
|~|取反|~是一元运算符，用来对一个二进制数按位取反，即将0变1，将1变0|
|<<|左移|用来将一个数的各二进制位全部左移N位，右补0|
|>>|右移|将一个数的各二进制位右移N位，移到右端的低位被舍弃，对于无符号数，高位补0|


- `i++`与`++i` / `i--`与`--i`
  - ++ 在前面叫做前自增（例如 ++a）。前自增先进行自增操作，再进行其他操作。
  - ++ 在后面叫做后自增（例如 a++）。后自增先进行其他操作，再进行自增操作。
  - 自增自减完成后，会用新值替换旧值，并将新值保存在当前变量中。
  - 自增自减只能针对变量，不能针对数字。

例子：
``` C
#include<stdio.h>

int main()
{
	int a = 5;
	printf("%d\n", ++a);
	printf("%d\n", a++);

	return 0;
}
```
- `sizeof`
  - 它以字节为单位返回运算对象的大小。
  - 对于实值，可省略括号，其他情况不能省略括号。
  - 根据代码的一致性，建议在所有情况下都不要省略括号。
``` C
#include<stdio.h>

int main()
{
	int  a = 5;
	printf("%d\n", sizeof a);
	printf("%d\n", sizeof int);	// 报错 
	printf("%d\n", sizeof(int));

	return 0;
}
```

# printf()和scanf()
这两个函数都采用格式化输入输出，每种数据类型都要使用它对应`转换说明`才能正常输入输出。例如整数要用`%d`，字符要用`%c`。这些符号称为`转换说明`，它们指定了如何把数据转换成可显示的格式。

### printf()
- 转换说明及其打印的输出结果

转换说明|输出
|-|-|
%a,%A|浮点数、十六进制数和p计数法(C99/C11)
%c|一个字符
%d|有符号十进制数
%e,%E|浮点数,e计数法
%f|浮点数，十进制计数法
%g,%G|根据数值不同自动选择%f或%e, %e格式在指数小于-4或者大于等于精度时使用
%i|有符号十进制整数(与%d相同)
%o|无符号八进制整数
%p|指针
%s|字符串
%u|无符号十进制数
%x,%X|使用十六进制数0f的无符号十六进制整数
%%|打印一个百分号

- 有时字符串比较长，需要放在多行
``` C
#include<stdio.h>

int main()
{
	printf("Do not believe what is passed from mouth; \
Do not believe rumors ; Do not believe the \
infallibility of texts") ;

	return 0;
}
```

### scanf()
scanf()函数使用空白（换行符、制表符和空格）把输入分成多个字段。在依次把转换说明和字段匹配时跳过空白。
> `%c转换说明`:它是一个例外，它会读取每个字符，包括空白。

- #### 格式字符串中的普通字符
scanf()函数允许把普通字符放在格式字符串中，这个时候要求，除空格字符外的普通字符必须与输入字符串严格匹配。例如
``` C
printf("%d,%d,%d", &a, &b, &c);
```
那么它对应的输入必须是这种格式
``` 
1,2,3
```

- #### 细化scanf()输入
![4](img/4.jpg)
- 假设scanf()根据一个%d转换说明读取一个整数。

scanf()函数每次读取一个字符，跳过所有空白字符，直至遇到第1个非空白字符才开始读取。scanf()不断地读取和保存字符，直至遇到非数字字符。如果遇到非数字字符，它便认为到了整数的末尾。然后scanf()把非数字字符放回输入。

- 如果第1个非空白字符(记为A)不是数字会怎么样？

scanf()将会停在那里，并把A放回输入中，不会把值赋给指定变量。程序在下一次读取输入时，首先读到的字符是A。如果程序只用了%d转换说明，那么scanf()就一直无法越过A读取下一个字符。另外，如果使用带了`多个`转换说明的scanf()，C规定在第1个出错出停止读取输入。

- %s:

scanf()会跳过空白，读取非空白字符，也就是说通过`%s`读取的字符串不含空白。需要注意的是，当scanf()把字符串放进指定数组中时，它会在字符序列的末尾加上`\0`。

- #### scanf()的返回值
scanf()函数返回成功读取的项数。如果没有任何读取项，且需要读取一个数字而用户却输入一个非数值字符串，scanf()便返回0。当检测到“文件结尾”时，会返回`EOF`。<br>
例子：
``` C
#include<stdio.h>

int main()
{
	char a[20];
	int sign, b, c;
	while(true)
	{
		sign = scanf("%s %d %d", a, &b, &c);
		printf("返回:%d\n", sign);
	}

	return 0;
}
```

由此可作为输入时的循环条件
``` C
while (~scanf("%d %d",&n,&m)) 等效于 while (scanf("%d %d",&n,&m) != EOF)
```

- #### 其他输入输出
  - getchar()
  - putchar()
  - gets()
  - puts()


# 分支语句
- #### if
> **形式：**
> ``` C
> if ( expression2 )
> 	statement1
> else if ( expression2 )
> 	statement2
> else
> 	statement3
> ```
> 如果expression1为真，执行statement1部分；如果expression2为真，执行statement2部分；否则，执行statement3部分

- 在写条件时，注意优先级。例如下面错误的判断是否为字母
``` C
if(ch >= 'a' && ch <= 'z' || ch >= 'A' && ch <= 'Z')
{
	printf("is alphabet")
}
```

- #### 三元运算符：? :
> 条件运算符需要3个运算对象，每个运算对象都是一个表达式。
> *expression1* ? *expression2* : *expression3*
> 如果*expression1*为真，整个条件表达式的值是*expression2*的值；否则，是*expression3*的值。

它可以与`if else`等效，例如
``` C
x = (y < 0) ? -y : y; 

// 等效于
if (y < 0)
	x = -y;
else
	x = y;
```

- #### 多重选择：switch语句
> **形式：**
> ``` C
> switch( expression ) 
> {
> 	case label1: statement1 // 使用break跳出switch
> 	case label2: statement2 
>	default: statement3
> }
> ```
> 可以有多个标签语句，default语句可选。<br>
> **注解：**<br>
> 程序根据expression的值跳转至相应的case标签处，然后，执行剩下的所有语句，除非执行到break语句进行重定向。expression和case标签都必须是整数值（包括char类型），标签必须是常量或完全由常量组成的表达式，如果没有case标签与expression的值匹配，控制则转至标有default的语句（如果有的话）；否则，将转至执行紧跟在wwitch语句后面的语句。

- EX：统计一段话中元音字母个数。
``` C
#include<stdio.h>

int main()
{
	int count = 0;
	char ch;
	while((ch = getchar()) != '\n')
	{
		switch(ch)
		{
			case 'a':
			case 'A':
				count++;
				break;
			case 'e':
			case 'E':
				count++;
				break;
			case 'i':
			case 'I':
				count++;
				break;
			case 'o':
			case 'O':
				count++;
				break;
			case 'u':
			case 'U':
				count++;
				break;
		}
	}
	printf("count: %d", count);

	return 0;
}
```


# 循环语句

### 入口条件循环
顾名思义，入口条件循环就是在循环的每次迭代之前检查测试条件，所以它有可能根本不执行循环体中的内容。
- #### while循环
> **形式:**
>  ``` C
>  while( expression )
> 		statement
>  ```
> 在*expression*部分为假之前，重复执行*statement*部分。

循环输入的例子：
``` C
#include<stdio.h>

int main()
{
	char ch;
	while(scanf("%c", &ch) != EOF)
	{
		if(ch >= '0' &&ch <= '9')
			printf("%c", ch);
	}

	return 0;
}
```
- **何为真假？**<br>
在C语言中这个很好判断，不为0的数就是`真`，即-1,-1000,1,100都为真。只有0为`假`。<br>布尔型的`True == 1`、`False == 0`。
	- EX.1：若成功输入，预测下面代码执行结果。
``` C
#include<stdio.h>

int main()
{
	int num, status;
	int sum = 0;

	status = scanf("%d", &num);

	while(status = 1)
	{
		sum += num;
		if(sum >= 10)
			status = 0;
	}
	printf("%d", sum);

	return 0;
}
```
出现了死循环，`while(status = 1)`实际上相当于`while(1)`，此时入口条件永为真。<br>
这种错误，程序在编译时，编译器一般不会报错（现代编译器会发出警告），为避免出现这种误用情况，经验丰富的程序员一般会把数写在等号左边，这样如果出现误写，在编译时会报错。
``` C
1 = status	// 语法错误
1 == status	// 返回真假
```

- **空语句**<br>
在C语言中，单独的分号表示空语句。有时程序员会故意使用带空语句的while语句，例如，假设你想跳过输入到第1个非空白字符或数字，可以这样写。
``` C
while(scanf("%d", &num) == 1)
	; 			// 跳过整数输入
```
防止误用空语句。以下是常见的几种误用，大括号中的语句仅执行了一次
``` C	
	int i = 0;
	while(i > 5);
	{
		i++;
		printf("%d", i);
	} /* 输出：1*/


	int i;
	for(i = 0; i < 5; i++);
	{
		printf("%d", i);
	} /* 输出：5*/


	int i = 5;
	if(i > 999);
	{
		printf("%d", i);
	} /* 输出：5*/
```

- #### for循环
for循环把初始化、测试和更新三个行为组合在了一处。
> **形式：**
> ``` C
> for ( initialize; test; update )
> 	statement
> ```
> 在*test*为假或0之前，重复执行*statement*。<br>
> **注解：**<br>
> for语句使用3个表达式控制循环过程，分别用分号隔开。initialize表达式在执行for语句之前只执行一次；然后对test表达式求值，如果表达式为真（或非零），执行循环一次；接着对update表达式求值，并再次检查test表达式。for语句是一种入口条件循环，即在执行循环之前就决定了是否执行循环。因此，for循环可能一次都不执行，statement部分可以是一条简单语句或复合语句。

- 输出1到200的奇数，十个为一行
``` C
#include<stdio.h>

int main()
{
	int i;
	for (i = 1; i <= 200; i+=2)
	{
		printf("%d", i);
		printf("%c", (i + 1) % 20 ? '\t': '\n' );
	}
	
	return 0;
}
```

- #### 逗号运算符
> 逗号运算符把两个表达式连接成一个表达式，并保证最左边的表达式最先求值，逗号运算符通常在for循环头的表达式中用于包含更多的信息。整个逗号表达式的值是逗号右侧表达式的值。

上个例子还可以这样写。
``` C
#include<stdio.h>

int main()
{
	int i, k;
	for (i = 1, k = 1; i <= 200; i+=2, k++)
	{
		printf("%d", i);
		if(k % 10 == 0)
			printf("\n");
		else
			printf("\t");
	}

	return 0;
}
```
- 防止误用逗号运算符<br>
举个例子，假如你正在给一个表示房价的变量赋值，它在书上表示的是`$295,500`，然后你在输入的时候，不小心把逗号也输入进去了。
``` C
houseprice = 259,500;
```
结果是houseprice的值被赋为了500。这不是语法错误，C编译器会将其解释为一个逗号表达式。以逗号为分隔，`500`成了一条语句，由于它位于表达式的最右侧，所有就是这个表达式的值。

- #### while or for
这两个循环可以做到互相等价，例如：
``` C
for(; test ; ){}

/* 等效于 */
while (test){}
```
``` C
初始化;
while( 测试 )
{
	其他语句
	更新语句
}

/* 等效于 */
for( 初始化; 测试 ; 更新 )
	其他语句

```
一般而言，当循环涉及初始化和更新变量时，用for循环比较合适，而在其他情况下用while循环更好。

### 出口条件循环：do while
出口条件循环，即在循环的每次迭代之后检查测试条件，这保证了至少执行循环体中的内容一次。
> **形式：**
> ```C
> do
> 	statement
> while( expression );
> ```
> 在*test*为假或0之前，重复执行statement部分

- 验证密码
``` C
#include<stdio.h>
#define PASSWORD 123456

int verify_password(int num)
{
	if(num == PASSWORD)
		return 0;
	else
		return 1;
}

int main()
{
	int password;
	do
	{
		printf("Please enter password:");
		scanf("%d", &password);
	}
	while(verify_password(password));
	printf("success!");

	return 0;
}
```

- #### 跳出循环
	- `continue`：**结束本次**循环，进行下一次循环
	- `break`：**终止**循环不再进行


### 函数
代码示例：
``` C
#include<stdio.h>

int max(int, int);	// 函数原型

int main()
{
	int a, b, num;
	scanf("%d %d", &a, &b);
	num = max(a, b);	// 函数调用
	printf("%d", num);

	return 0;
}

int max(int a, int b)	// 函数定义
{
	return a > b ? a : b;	// 返回int类型的值
}
```
- 什么是函数签名？<br>
函数的返回类型和形参列表构成了函数签名。因此函数签名指定了传入函数的值的类型和函数值的类型。

- 函数原型的作用<br>
之所以使用函数原型，是为了让编译器在第1此执行到该函数之前就知道如何使用它。<br>既然是告知编译器如何使用它，那么肯定有等效的方法能省略它。例如上面的代码
``` C	
int max(int a, int b)	// 函数定义
{
	return a > b ? a : b;	// 返回int类型的值
}

int main()
{
	int a, b, num;
	scanf("%d %d", &a, &b);
	num = max(a, b);	// 函数调用
	printf("%d", num);

	return 0;
}
```
只需要在调用子函数之前，让编译器知道它的存在即可。

- 同名函数<br>
在支持ANSI C的编译器下，可以使用相同的名称命名多个函数，只要它的函数签名不同即可。注意在g++编译器下，不允许这样的操作。
举个例子：
``` C
#include<stdio.h>

int max(int, int);	// 函数原型
char max(char, char);

int main()
{
	int a, b;
	char ch1, ch2;

	scanf("%d %d", &a, &b);
	printf("max: %d\n\n", max(a, b));

	getchar(); // 读取换行
	scanf("%c %c", &ch1, &ch2);
	printf("char is %c and %c\n", ch1, ch2);
	printf("max: %c\n", max(ch1, ch2));

	return 0;
}

int max(int a, int b)	// 函数定义
{
	return a > b ? a : b;	// 返回int类型的值
}

char max(char a, char b)
{
	return a > b ? a : b;
}
```
- #### 了解：与指针相关的运算符
> **地址运算符：&**
> **注解**：后跟一个变量名时，&给出该变量的地址
> **示例**：&house表示变量house的地址。

> **地址运算符：***
> **注解**：后跟一个指针名或地址时，*给出储存在指正指向地址上的值。
> **示例**：<br>
> ``` C
> house = 22;
> ptr = &house;	// 指向house指针
> value = *ptr; // 把ptr指向的地址上的值赋给value
> ```

- #### 引用
函数的形参如果是地址，可以称为引用变量。此时修改形参的值，将会直接影响实参的值。
举个例子
``` C
#include<stdio.h>

void find_max(int a, int b, int &max)
{
	max = a > b ? a : b;
}

int main()
{
	int a, b, max;
	scanf("%d %d", &a, &b);
	find_max(a, b, max);
	printf("max: %d\n", max);

	return 0;
}
```
在子函数中给`max`变量赋值，也会直接影响main函数中的max的值。

- #### 返回指针
``` C
#include<stdio.h>

char *input()
{
	char str[20];
	scanf("%s", str);
	return &str[0];
}

int main()
{
	char *str = input();
	printf("%s", str);

	return 0;
}
```
- 思考：如果一个子函数，有多个同类型的值要返回怎么办呢？

<div STYLE="page-break-after: always;"></div> 



## 数组

### 为什么要用数组

#### 一个典型的问题

+ 一次性获取用用户输入的十个整数，然后对每一个数据做平方处理然后输出，一般情况下，既然要一次获取10个整数,那么就必须定义十个变量来储存这是个数据，代码看起来已经非常的麻烦了，如果数量更大的话，就会更麻烦了，比如以下代码

```c
int main()
{
    int i1,i2,i3,i4 ... i10;
    scanf("%d%d%d ... %d",&i1,&i2,&i3 ... &i10);
    i1 *= i1;
    i2 *= i2;

    .....

    printf("%d%d%d.....%d",i1,i2,i3....i10);
}
```

+ 为了对这种大量数据进行处理，C语言引进了数组

### 数组的定义和使用

#### 定义数组

+ 在 C 中要声明一个数组，需要指定元素的类型和元素的数量，使用数组前，需要先定义数组，定义数组的格式如下：

```c
数据类型 数组名 [整形常量表达式], ...
type arrayName [ arraySize ];
```

+ 数组由数据类型相同的一系列元素组成，需要使用该数组时，通过声明数组告诉编译器数组中含有多少元素和这些元素的类型，编译器根据这些信息正确的创建数组 ，例如：

```c
int main()
{
    float candy[365] /*内含有365个float类型元素的数组 */
    char code[20];  /*内含有20个char类型元素的数组 */
    int book[50];   /*内含有50个int类型元素的数组 */
}
```

#### 初始化数组

+ 数组的声明并不是声明一个个单独的变量，比如 number0、number1、...、number99，而是声明一个数组变量，比如 numbers
+ 如下代码所示，用以逗号分割的数值列表 (用花括号括起来) 来初始化数组

```c
int main()
{
    int numbers[8] = {1,2,4,5,7,9,12,435};
}
```

+ 大括号 { } 之间的值的数目不能大于我们在数组声明时在方括号 [ ] 中指定的元素数目。如果你省略掉了数组的大小，数组的大小则为初始化时元素的个数。因此，如果执行以下的代码 将创建一个数组，它与前一个实例中所创建的数组是完全相同的。

```c
double balance[] = {1000.0, 2.0, 3.4, 7.0, 50.0};
```

创建完的数组balance在内存中的结构：

<div align="center">

![array_presentation.jpg](https://upload-images.jianshu.io/upload_images/9140378-1c77919410172241.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

</div>

+ 或者利用数组的下标进行赋值

```c
//下面是一个为数组中某个元素赋值的实例：

numbers[4] = 50.0;

//下面是为数组的所有元素进行赋值

int main()
{
    int num[5];
    for(int i = 0;i < 4;i ++)
        num[i] = i + 1;
}
```

创建完的数组的结构：

<div align="center">

![train4.png](https://upload-images.jianshu.io/upload_images/9140378-67efb5f61143edf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/440)

</div>

#### 通过数组下标访问数组的数值

+ 如下图所示，使用 numbers[0]、numbers[1]、...、numbers[99] 来代表一个个单独的变量。数组中的特定元素可以通过索引访问。所有的数组都是由连续的内存位置组成。最低的地址对应第一个元素，最高的地址对应最后一个元素。，数组的首位置的下标一定是0,下标的最大值一定是数组的长度 - 1

<div align="center">

![train5.jpg](https://upload-images.jianshu.io/upload_images/9140378-a6be205a10460f5b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

</div>

+ 数组边界

在访问数组元素时，要防止数组下标超出边界，也就是说，必须确保下标时有效的值，加入有下面的声明：

```c
int bao[20];
```

那么在访问该数组时，要确保程序中使用的数组下标在 0 ~ 19 的范围内，因为编译器不会检查出这种错误，也就是说，编译运行照常通过，但看起来运行的结果很奇怪
假设我们访问数组下标以外的数值，例如：

```c
#include "stdio.h"
int main()
{
    int lihao[3] = {1,2,3};
    for(int i =0;i < 7;i ++)
        printf("%d\n",lihao[i]);
    return 0;
}
/*
1
2
3
3
11692832
0
4199400
*/
```

+ C是相信程序员能正确的编写程序，这样C可以运行的更快，所以访问数组时C是不检查边界的，但不是所有程序员可以做到这一点，所以才出现了这个数组下标越界的问题，所以建议大家在声明数组的时候使用符号常量来表达数组的大小

```c
#define SIZE 4;
int main()
{
    int arr[SIZE];
    for(int i = 0;i < SIZE;i ++)

    ...

    return 0;
}
```

#### 数组的简单应用

两个经典的排序

+ 冒泡排序

<div align="center">

![train7.png](https://upload-images.jianshu.io/upload_images/9140378-5eb8e56677da3aa4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/350)

</div>

+ 比较相邻的元素。如果第一个比第二个大，就交换他们两个，就像把大的像泡泡一样“冒”到数组后面去
+ 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。
+ 针对所有的元素重复以上的步骤，除了最后一个。
+ 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

```c
int main()
{
    int i = 0,j = 0;
    int a[10] = {3,1,4,1,5,9,2,6,5,4};
    for(i = 0;i < 9;i ++)
    {
        int temp = 0;
        for(j = 0;j < 9 - i;j ++)
        {
            if(a[j] > a[j + 1])
            {
                //将位置 j + 1 的数与位置j的数进行交换
                temp = a[j + 1];
                a[j + 1] = a[j];
                a[j] = temp;
            }
        }
    }
    for (i = 0;i < 10;i ++)
        printf("%d ",a[i]);
    return 0;  
}

/*
1 1 2 3 4 4 5 5 6 9
*/
```

[动态演示 -- 冒泡排序算法可视化](https://visualgo.net/zh/sorting)

+ 选择排序

<div align="center">

![train8.png](https://upload-images.jianshu.io/upload_images/9140378-0c2f621c106a60ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/380)

</div>

+ 选择排序（从小到大）的基本思想是，首先，选出最小的数，放在第一个位置；然后，选出第二小的数，放在第二个位置；以此类推，直到所有的数从小到大排序。
+ 在实现上，我们通常是先确定第i小的数所在的位置，然后，将其与第i个数进行交换

```c
#include "stdio.h"
int main()
{
    int i = 0,j = 0;
    int temp = 0;
    int a[10] = {3,1,4,1,5,9,2,6,5,4};
    for(i = 0;i < 9;i ++)
    {
        int pos = 0;
        for(j = 1;j < 10 - i;j ++)
            if(a[pos] < a[j])
                pos = j;
        if(pos != 9 - i)
        {
            temp = a[9 - i];
            a[9 - i] = a[pos];
            a[pos] = temp;
        }
    }
    for (i = 0;i < 10;i ++)
        printf("%d ",a[i]);
    return 0;  
}
/*
1 1 2 3 4 4 5 5 6 9
*/
```

[动态演示 -- 选择排序算法可视化](https://visualgo.net/zh/sorting)

### 多维数组

#### 多维数组的声明

C 语言支持多维数组。多维数组声明的一般形式如下：

```c
type name[size1][size2]...[sizeN];
```

例如，下面的声明创建了一个三维 5 . 10 . 4 整型数组：

```c
int threedim[5][10][4];
```

### 二维数组

#### 初始化二维数组

+ 初始化可以使用一个 { } 里面包含有多个数据的方式，多个数据使用逗号进行分隔,例如：

```c
数据类型 数组名[整型常量][整形常量] = { {数据1,...} , {数据2,...}, {数据3,...}, ...};

int a[3][4] = {
    {0, 1, 2, 3}, /*  初始化索引号为 0 的行 */
    {4, 5, 6, 7},  /*  初始化索引号为 1 的行 */
    {8, 9, 10, 11}  /*  初始化索引号为 2 的行 */
};
```

+ 数组的第一个下标可以直接由 “{ }” 里面的包含的 “{ }” 个数决定，所以完全可以省略第一个下标：

```c
int a[][4] = { {0, 1, 2, 3}, {4, 5, 6, 7}, {8, 9, 10, 11} };
```

+ 虽然二维数组在概念上是二维的，有行和列，但在内存中所有的数组元素都是连续排列的，它们之间没有“缝隙”,以下面的二维数组 a 为例：

```c
int a[3][4] = { {0, 1, 2, 3}, {4, 5, 6, 7}, {8, 9, 10, 11} };
```

+ 但在内存中，a 的分布是一维线性的，整个数组占用一块连续的内存：

<div align="center">

![train3.jpg](https://upload-images.jianshu.io/upload_images/9140378-753dc9d279810b55.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

</div>

+ C语言中的二维数组是按行排列的，也就是先存放 a[0] 行，再存放 a[1] 行，最后存放 a[2] 行；每行中的 4 个元素也是依次存放。

<div align="center">

![train11.jpg](https://upload-images.jianshu.io/upload_images/9140378-7445afc38122c969.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

</div>

+ 数组 a 为 int 类型，每个元素占用 4 个字节，整个数组共占用 4×(3×4) = 48 个字节。
+ C语言允许把一个二维数组分解成多个一维数组来处理。对于数组 a，它可以分解成三个一维数组，即 a[0]、a[1]、a[2]。每一个一维数组又包含了 4 个元素，例如 a[0] 包含 a[0][0]、a[0][1]、a[0][2]、a[0][3]

+ 假设数组 a 中第 0 个元素的地址为 1000，那么每个一维数组的首地址如下图所示：

<div align="center">

![train9.png](https://upload-images.jianshu.io/upload_images/9140378-f382b296f8bf9418.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

</div>

#### 为二维数组赋值并访问二维数组的元素

+ 二维数组中的元素也是通过使用下标（即数组的行索引和列索引）来访问的。例如：

```c
int val = a[2][3];
```

+ 上面的语句将获取数组中第 3 行第 4 个元素。您可以通过上面的示意图来进行验证。让我们来看看下面的程序，我们将使用嵌套循环来处理二维数组：

首先定义一个二维数组

```c
/* 一个带有 3 行 4 列的数组 */
int a[3][4];
```

二维数组的赋值

+ 使用循环的嵌套来从键盘录入数据

```c
int i, j;
for(i = 0;i < 3;i ++ )
{
    for(j = 0;j < 4;j ++)
    {
        scanf("%d",&a[i][j]);
    }
}
```

<div align="center">

![train12.jpg](https://upload-images.jianshu.io/upload_images/9140378-a9894652810c62b5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

</div>

二维数组的输出

+ 同样我们使用循环的嵌套来从输出数据

```c
//输出具体数值
for ( i = 0; i < 3; i++ )
{
    for ( j = 0; j < 4; j++ )
        printf("%d ",a[i][j]);
    printf("\n");
}
/*
1 2 3 4
3 4 5 6
5 6 7 8
*/
    for ( j = 0; j < 4; j++ )
        printf("a[%d][%d] = %d\n", i,j, a[i][j]);
/*
a[0][0] = 1
a[0][1] = 2
a[0][2] = 3
a[0][3] = 4

a[1][0] = 3
a[1][1] = 4
a[1][2] = 5
a[1][3] = 6

a[2][0] = 5
a[2][1] = 6
a[2][2] = 7
a[2][3] = 8
    */
}
```

#### 多维数组的使用

从前面的学习中，大家应该都知道了，数组的维数与定数组时，数组的整型常量表达式，即下标的个数有关

+ 含有一个下标的是一维数组，包含两个下标的二维数组，很容易就可以推测出多维数组是如何定义并初始化的，如定义一个 int 型的三维数组

```c
int num[2][4][2];
```

+ 上面所定义的三维数组的储存单元数应该是：2 * 4 * 2，即16 个储存单元，无论是有几个下标的多维数组，都可以看成是数组不断嵌套的结果,而且多维数组在程序的编写中不经常用到
+ 接下来通过一个实例来让大家熟悉一下多维数组

```c
int mian()
{
    int address[3][6][4];
    for(int i = 0;i < 3;i ++)
    {
        printf("第%d栋楼\n\n"，i);
        for(int j = 0;j < 6;j ++)
        {
            printf("第%d层楼"，6 - j);
            for(int k = 0;k < 4;k ++)
            {
                address[i][j][k] = k + 1;
                printf("%d",address[i][j][k]);
            }
        }
    }
}
```

## 指针

### 指针及其使用

#### 初识指针

指针是什么？

首先理解 " & " 和 " * " ( 取地址和指针运算符 )

```c
int  var1;
char var2[10];
//打印出地址
printf("var1 变量的地址： %p\n", &var1  );
printf("var2 变量的地址： %p\n", &var2  );
/*
var1 变量的地址： 0x7fff5cc109d4
var2 变量的地址： 0x7fff5cc109de
*/
```

```c
scanf("%d",&i);//传入一个地址
//scanf的函数原型
int scanf(const char * restrict format,...);
```

如何定义指针变量？

+ 指针本身是一个变量，它存储的是数据在内存中的地址而不是数据本身的值。它的定义如下:

```c
数据类型* 变量名  或  数据类型 *变量名 //只是*的位置不同而已

int *pointer;
char *name;
```

+ 这样就定义好了两个指针变量，int 和 char 表示该这两个指针变量指向的数据类型，*表示这是指针变量。

+ 指针变量的初始化：每一个变量都有一个内存位置，每一个内存位置都定义了可使用连字号（&）运算符访问的地址，它表示了在内存中的一个地址,

![train13.png](https://upload-images.jianshu.io/upload_images/9140378-ccd065db4a4c4091.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640)

```c
 int a = 10,*p;  
 p = &a
 int a = 10;
 int *p = &a;
```

+ 首先我们可以理解 int * 这个是要定义一个指针p，然后因为这个指针存储的是地址所以要对a取地址(&)将值赋给指针p，也就是说这个指针p指向a。
+ 可能会对对这两种定义方法感到迷惑，其实他俩的意思是一样的。第一种定义方法定义了int型的变量 a 和 指针 p ，然后将 a 的地址赋给 p
+ 第二种是在定义指针p的同时将 a 的地址赋给指针 p。我们姑且理解为" int * "是定义指针的标志。

在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个 NULL 值是一个良好的编程习惯。赋为 NULL 值的指针被称为空指针。NULL 指针是一个定义在标准库中的值为零的常量

```c
int  *ptr = NULL;
printf("ptr 的地址是 %p\n", ptr  );
return 0;
/*
结果：
ptr 的地址是 0x0
*/
```

#### 指针有什么用

这样我们就可以通过*p来找到指针所指向的变量a的地址，然后对地址中的值(值是10)进行操作。

```c
printf("%p",p)   //结果是一个地址(p指向的变量a的地址)。
printf("%d",*p)  //结果是10，变量a的值。
printf("%d",&p)  //结果是一个地址(指针p的地址，因为指针也是一个变量自己也有地址的)
```

题目：

使用指针交换两个整数变量的值，并写成函数形式，即实现void swap(int *a, int *b)函数

#### 指针运算

指针就是地址，地址在内存中也是以数的形式存在，所以指针也能做加法，减法，比较等运算

```c
int a = 5;
int *i = &a;
printf("%p\n",i);
i ++;
printf("%p\n",i);
i -= 2;
printf("%p\n",i);
return 0;
/*
000000000062FE44
000000000062FE48
000000000062FE40
*/
```

### 指向数组的指针

#### 指向一维数组的指针

为指针赋数组数据的地址

+ 数组的每个数据都保存在一个储存单元里面，只要是储存单元就会有地址，既然指针变量的储存单元可以保存地址，那么就可以用指针保存数组储存单元的地址

```c
int *p_i = NULL;   //定义指针变量
int num[5] = {1，2，3，4，5};
for(int i = 0;i < 5;i ++)
{
    p_i = &num[i];  //先让指针指向想要输出的数据
    printf("%d ",*p_i); //通过指针输出数组数据
}
```

使用数组名为指针赋值

+ 对于下面的数组和指针

```c
int num[5] = {1,2,3,4,5};
int *p_i;
```

+ 为指针赋予第一个数组数据的地址的方式为：

```c
p_i = &num[0];
```

+ 其实还可以写成下面的形式,直接将数组名赋予指针，指针需要储存的数据就是地址，而 num 就代表的是数组的首地址

```c
p_i = num;
```

指向数组的指针的加减运算,  -- 数组的另外一种遍历方式

![train14.png](https://upload-images.jianshu.io/upload_images/9140378-29428f6b9449104d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

+ 对于*(num + i), num 是数组的首地址，指向数组的首元素，而num + i 则便是数组的第 i 个元素的地址，再加上指针运算符* 就得到了该元素的值
+ array每次加一的时候，它自己的值都会增加sizeof(int)，加 i 的时候就增加i *sizeof ( int )

```c
int num[5] = {2,4,6,8,10};
for(int i = 0;i < 5;i ++)
{
    //通过数组下标遍历数组
    printf("%d",num[i]);
    //通过指针变量遍历数组
    printf("%d",*(num + i));
}
```

#### 指向二维数组的指针

跟一位数组同样的道理

```c
int num[3][2] = {{1,2},{3,4},{5,6}};
int *p_i = &num[0][0];
```

+ 不过在这里要注意的是，不能为指针直接赋予二维数组的数组名，即上面的代码不能写成: int *p_i = num;

<div align="center">

![train9.png](https://upload-images.jianshu.io/upload_images/9140378-261b27c28b07feab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

</div>

+ 假设定义一个二维数组：num[m][n];一个指针p指向了这个二维数组的首地址，那么对于数组的数据 num[i][j](0 <= i < m,0 <= j < n) ，指针变量p要想指向这个数据,那么指针变量 p = p + n * i + j;

```c
double arr[4][3] = {
    {78.4,72.1,41.2},
    {56.4,12.4,45.1},
    {12.5,14.6,20.4},
    {23.5,34.6,67.8}
}
double *p_d = &arr[0][0]; //指针变量的类型必须要跟数组类型一致
printf("二维数组中arr[3][2]位置上的数据为：%6.11f\n",*(p_d + 3 * 3 + 2));
```

### 保存指针的数组

<div align="center">

![train15.png](https://upload-images.jianshu.io/upload_images/9140378-f25fae78ce0cba15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

</div>

从名字的定义上来看，数组元素全为指针的数组就称为指针数组。

+ 一维指针数组的定义形式为： 类型名 *数组标识符[数组长度]
例如，一个一维指针数组的定义：

```c
int *ptr_array[10]
```

+ 因为[]比*的优先级高，所以也可以看成是*(ptr_array[10])，括号里面ptr_array[10]表示的是一个长度为10的数组，然后括号外面的* 说明数组的元素类型是 int* 的指针类型

+ 看一个指针数组的例子

```c
int main()
{
  int a = 16, b = 932, c = 100;
  //定义一个指针数组
  int *arr[3] = {&a, &b, &c};
  printf("%d %d %d\n", *arr[0], *arr[1], *arr[2]);
  return 0;
}
/*
16 932 100
*/

```

因为数组arr里面的元素都是指针，所以在声明指针数组的时候，把a,b,c的地址&啊，&b,&c传进去了，在输出的时候先通过 数组下标得到数组内的指针，即a,b,c的地址，然后，再通过 运算符 * 将数据取出

### 数组指针

定义方式

```c
datatype (*ptr)[length]
```

如果一个指针指向了数组，就称它为数组指针,例如：

```c
int a[4][3] = {{0,2,3},{1,5,6},{2,3,4},{7,8,9}};
```

在概念上他是像这种矩阵的样子：

```c
0 2 3
1 5 6
2 3 4
7 8 9
```

但实际上他在内存中是链式的：

```c
0 2 3 1 5 6 2 3 4 7 8 9
```

我们可以将这个二维数组分解成多个一维数组,a[0]包括a[0][0]、
a[0][1]、a[0][2] 三个元素

```c
     a[][0]  a[][1]  a[][2]
a[0]    0       2       3
a[1]    1       5       6
a[2]    2       3       4
a[3]    7       8       9
```

这里的 a 就是那四个一位数组的组名，接着我们定义一个 数组指针

```c
int (*p)[4] = a;
```

括号里面的*代表 p 是一个指针，[4] 代表这个 指针 p指向了类型为 int[4] 的数组

下面使用数组指针来遍历一遍二维数组

+ p指向数组a的开头，就是指向第0行元素，p + 1 代表的是数组中的第一行元素
+ 那么 *(p + 1) 就表示的是 数组a 的第一行元素，是多个数据
+ *(p + 1) + 1 则表示的是第一行的第一个数据的地址
+ ((p+1)+1)表示第一行的第一个数据的值

### 指针在函数中的应用

#### 指针作为函数参数

```c
#include "stdio.h"

void add_five(int *);
int main()
{
    int i = 10;
    add_five(&i);
}

void add_five(int *a)
{
    *a = *a + 5;
}
```

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