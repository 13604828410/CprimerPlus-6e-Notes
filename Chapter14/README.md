## ## 第十四章 结构和其他数据形式

### 👉【[复习题](./复习题.md)】【[编程练习题](./编程题.md)】
C语言中，提供结构变量 可以提高表示数据的能力，创造新的形式。

### 示例问题：创建图书目录
打印图书目录，包括：书名、作者、价格。
```cpp
#include<stdio.h>
#include<string.h>
char *s_gets(char * st,int n);
#define MAXTITL 41; // 书名最大长度
#define MAX AUTL 31; // 作者名字的最大长度

struct book{
    char title[MAXTITL];
    char author[MAXAUTL];
    float value;
};//结构模版结束

int main(void)
{
    struct book library; // 把library声明为一个book类型的变量
    printf("Please enter the book title.\n");
	s_gets(library.title, MAXTITL);
	printf("Now enter the author.\n");
	s_gets(library.author, MAXAUTL);
	printf("Now enter the value.\n");
	scanf("%f", &library.value);
	printf("%s by %s: $%.2f\n", library.title, library.author, library.value);
	printf("%s: \"%s\"($%.2f)\n", library.author, library.title, library.value);
	printf("Done.\n");
    
    return 0;
}

char *s_gets(char * st,int n)
{
    char * ret_val;
	char * find;
	ret_val = fgets(st, n, stdin);
	if (ret_val)  
	{
		find = strchr(st, '\n');//查找换行符
		if (find)              //如果地址不是NULL
			*find = '\0'; //将换行符换成'\0'
		else
			while (getchar() != '\n')  //处理输入行剩余的字符
				continue;
	}
	return ret_val;
}
```
结构的3个技巧
> - 为结构建立一个格式或样式。
> - 声明一个适合该样式的变量。
> - 访问结构变量的各个部分。

### 2. 建立结构声明
- `结构声明（structure declaration）`描述了一个结构的组织布局。

- 在结构声明，用`一对花括号`括起来的是`结构成员列表`。每个成员都可以是任意一种C的数据类型。

- 右花括号后面的分号是声明必需，表示结构布局定义结束。

- 结构声明可以放在所有函数的外部，也可以放在一个函数定义的内部，但是如果把声明放在函数内部，它的标记只局限于该函数内部使用。

- 结构的标记名是可选的，但是在一处定义结构布局，在另一处定义实际的结构变量，必须使用标记。

### 3. 定义结构变量
结构的含义：
> - 结构布局告诉编译器如何表示数据，但不让编译器为数据分配空间。
>
> - 创建另一个结构变量。

#### 3.1 初始化结构
让每个成员的初始化项独占一行。目的：提高代码的可读性，对编译器而言，只需要用逗号分隔各成员的初始化项。
- 初始化一个结构变量（ANSI之前，不能自动变量初始化结构，ANSI之后可以用任意存储类别）与初始化数组的语法类似。

- 使用在一对花括号中括起来的初始化列表进行初始化，各初始化项用逗号分隔。

**⚠️注意：初始化结构和类别存储期**
- 如果初始化一个静态存储期的结构，初始化列表中的值必须是常量表达式。

- 如果是自动存储期，初始化列表中的值可以不是常量。

#### 3.2 访问结构成员
- 使用结构成员运算符 ————> `点(.)` 访问结构中的成员。
```cpp
/*访问library中的title部分*/
library.title = "C Primer Plus";
```
- `点(.)` 比 `地址符(&)` 优先级高。
#### 3.3 结构的初始化器
- C99和C11为结构提供了指定`初始化器（也叫标记化结构初始化语法）`，结构的指定初始化器使用`点运算符`和`成员名（而不是方括号和下标）标识`特定的元素。
```cpp
struct book gift = {
	.price = 69.00,
	.author = "Stephen Prata",
	.title = "c primer plus"
};
```
- 在指定初始化器后面的`普通初始化器`，为指定成员后面的成员提供初始值。

- 指定成员的最后一次赋值才是它实际获得的值。

### 4. 结构数组

#### 4.1 声明结构数组
声明结构数组和声明其他类型的数组类似。
```cpp
/*把library声明为一个内含MAXBKS个元素的数组*/
struct book library[MAXBKS];
```
> 数组名library不是结构名，是数组名。数组中的每个元素都是struct book类型的结构变量。

#### 4.2 标识结构数组的成员
为了表示结构数组中的成员，采用访问单独结构的规则：在结构名后面加一个点运算符，再在点运算符后面加上成员名。如：
```cpp
library[0].price  /*第1个数组元素与price相关联*/
library[4].title  /*第5个数组元素与title相关联*/
```
> ⚠️注意：***数组下标紧跟在数组名（如library后面），不是成员名后面。***

#### 4.2 嵌套结构
嵌套结构：在一个结构中包含另一个结构。
如果访问嵌套结构的成员，需要使用两次点运算符。
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#define LEN 20
const char * msgs[5] =
{
	" Thank you for the wonderful evening, ",
	"You certainly prove that a ",
	"is a special kind of guy. We must get together",
	"over a delicious ",
	" and have a few laughs"
};

struct names {
	char first[LEN];
	char last[LEN];
};
 
struct guy {
	struct names handle;  //嵌套结构，结构里包含另一个结构
	char favfood[LEN];
	char job[LEN];
	float income;
};
 
int main(void)
{
	struct guy fellow = {
		{"Ewen", "Villard"},
		"grilled salmon",
		"personality coach",
		68112.00
	};
	printf("Dear %s,\n\n", fellow.handle.first); //使用嵌套结构，先使用.得到name，再.得到first
	printf("%s%s.\n", msgs[0], fellow.handle.first); 
	printf("%s%s\n", msgs[1], fellow.job);
	printf("%s\n", msgs[2]);
	printf("%s%s%s", msgs[3], fellow.favfood, msgs[4]);
	if (fellow.income > 150000.0)
		puts("!!");
	else if (fellow.income > 75000.0)
		puts("!");
	else
		puts(".");
	printf("\n%40s%s\n", " ", "See you soon,");
	printf("%40s%s\n", " ", "Shalala");
 
	return 0;
}
```

### 5. 指向结构的指针
使用指向结构的指针的理由：
- 指向结构的指针通常是结构本身更容易操控（指向数组的指针比数组本身更容易操控）。
- 早期C实现，结构不能作为参数传递给函数，可以传递指向结构的指针。
- 能传递一个结构，传递指针通常更有效率。
- 一些用于表示数据的结构中包含指向其它结构的指针。

#### 5.1 声明和初始化结构指针
例子：
```cpp
struct guy *him;
```
与数组不同的是，~~结构名并不是结构的地址~~，因此在结构名前加上`&运算符`。

#### 5.2 用指针访问成员
两种方式：
- 使用 `->` 运算符
	- 如果 him==&barney ,那么 him->income 即是 barney.income
	- 如果 him==&fellow[0] , 那么 him->income 即是 fellow [0].income


- 用`运算符`（必须使用`圆括号`，因为`(.)运算符`比`(*)`运算符的优先级高）。
```cpp
barney.income == (*him) . income == him->income //假设him== &barney
fellow [0].income == (*him) .income
```
有些系统，**一个结构的大小可能大于它各成员大小之和**。

### 6. 向函数传递结构的信息
函数的参数把值（都是数字---> int类型，float类型、ASCII字符码、地址）传递给函数。

#### 6.1 传递结构成员
只要结构成员是一个具有单个值的数据类型（即int及其相关类型、char、float、double或指针），可作为参数传递给接受该特定类型的函数。
```cpp
/*把结构成员作为参数传递*/
#include <stdio.h>
#define FUNDLEN 50

struct funds {
	char   bank[FUNDLEN];
	double bankfund;
	char   save[FUNDLEN];
	double savefund;
};

double sum(double, double);

int main(void)
{
	struct funds stan = {
		"Garlic-Melon Bank",
		4032.27,
		"Lucky's Savings and Loan",
		8543.94
	};

	printf("Stan has a total of $%.2f.\n",
		sum(stan.bankfund, stan.savefund));
	return 0;
}

/* adds two double numbers */
double sum(double x, double y)
{
	return(x + y);
}
```

#### 6.2 传递结构的地址
必须使用`&运算符`来获取`结构的地址`，结构名只是其地址的别名。






