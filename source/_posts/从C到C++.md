---
title: 从C到C++
date: 2024-08-01 21:15:00
updated: 2024-08-01 21:15:00
---

<meta name="referrer" content="no-referrer"/>

# 从C到C++

## C++

1、所有c语言文件都是标准的C++文件

2、文件后缀为.cpp 编译器程序为g++



名字控制：重名问题

<!-- more -->

### 命名空间：namespace  :: 域解析符

```
namespace A
{
    int a = 10;
    void func()
    {
        printf("namespace A\n");
    }
}
int main()
{
    A::func();
    return 0;
}
```

using :使后面跟着的描述符生效

```
using namespace A::B;  //使命名空间A::B生效
```

\#include <iostream>  //C++输入输出

```
std::cin>>a;
std::cout<<a<<std::endl
```

\#include<cstring>  //原c语言的库  标准命名空间下 在C++中调用要在前面加一个c

\#include<cstdlib>

\#include<ctime>

std: standrad :C++标准命名空间

```
std::cout << "hello\n";
```

std::endl  :1、输出一个换行  2、刷新输出流

```
std::cout << "hello\n"<<std::endl;
```

### struct加强：

​					1、结构体名称可以单独作为类型出现

​           		 2、结构体里可以定义函数

引进了bool类型：  bool成为了内嵌入编译器的关键字

​							1、占一个字节内存

​							2、赋任何非0值都是1

左值=右值

左值：有内存，可修改的值

右值：没有内存，不可修改的值

三目运算符的加强：  ？ ：

​				表达式返回的是左值

### const：

```
    const int a=10;
    int *p =(int *) &a;
    *p = 20;
    std::cout <<&a<<" "<< p << std::endl;
```

a的值在符号表里，取地址时分配内存，但通过a去找，一定是去符号表里找，来保证a的常量属性

const是一个真正的常量

### 引用：(变量的别名)

1、引用不能单独出现，不能单独定义，一定要初始化

2、引用一经定义，不会再修改朝向

定义和指针相似：int &a;

```
    int b = 10;
    int &a=b;
    a = 20;
```

此时修改a相当于修改b

3、其本质就是指针常量

```
int &b=a;
int *const b=&a;
```

所以又称左值引用，没有办法传递右值

```
int &b=10;  //该定义不可以
```

常引用：左值引用无法传递右值，引入常引用 （常引用不能修改）

```
const int &b=10；
```

### 函数：

内联函数：inline 会在函数调用的地方进行代码替换

目的：提高程序执行的效率

用内联函数替换宏函数  

```
inline int add(int a,int b)
{
    return a + b;
}
```

注意事项

1、定义在类内和结构体内的函数自动变为内联函数

2、内联的规则：

​					（1）不能有循环（for while 递归）

​					（2）不能存在多个判断分支

​					（3）函数体不能太长

​					（4）不存在任何对函数的取地址操作

### 默认参数：

调用函数时，不传参就用默认值

默认参数定义在函数声明中时，定义中不能再出现

```
声明：int func(int a,int b,int c=0)；
定义：int func(int a,int b,int c)
{
    return a + b + c;
}
```

1、默认参数都放在最右边

2、出现默认参数时，后面的都必须是默认参数

### 占位参数：

```
int func(int a,int)
{
    
}
int main()
{
    func(1, 2) ;
    return 0;
}
```

### 函数重载：

不同的函数功能可以用相同的函数名

```
func(int a)  ->func    //c语言中
func(int a)  ->_int_func  //c++中
func(char a)  ->_char_func  //c++中
func(int a,int b)  ->_int_int_func //c++中
func(char a,int b)  ->_char_int_func //c++中
func(int b，char a)  ->_int_char_func //c++中
```

重载的规则：

1、参数数量不一样

2、参数类型不一样

3、参数顺序不同

4、返回值不同能不能构成重载？ 不能！！！（返回值并没有被当成名字放进去）



 nullptr : 空指针    在C++中不能用NULL，只能用 nullptr

在重载中，宏定义没有数据类型

建议在C++中用 const 替代 #define 定义常量

```
#define Max 1024
const int Max = 1024;
```

C语言：弱类型

C++：强类型

### 堆上的内存管理：（new、delete）

#### 新建堆上空间：new

```
    int *a = new int;//C++
    int *a1 = (int *)malloc(sizeof(int));//C语言
    
    int *a = new int[10];//C++定义堆上数组空间
    
    int **a = new int*[3];//C++定义堆上二维数组空间
    for (int i = 0; i < 2;i++)
    {
        a[i] = new int[3];
    }

```

1、自行计算所需的内存大小

2、返回的指针不需要强转

3、不需要判断返回值为空

如果申请失败，会抛出异常，如果异常不被捕获，默认终止程序

#### 释放内存：delete

```
    delete a;
    delete []a;//C++释放堆上数组空间
    
    for (int i = 0; i < 2;i++)//C++释放堆上二维数组空间
    {
        delete[] a[i];
    }
```

### 字符串：

   #include<string>//调用string库

定义：std：：string  s;

```
    std::string s="hello";//创建字符串
    s += "world";//直接在hello后拼接字符串
    std::string s2 = s + ",jack";//直接在s后拼接字符串
    std::cout << s2 << std::endl;
    std::cout << s2.size() << std::endl;//直接计算字符串的长度
    std::cout << (s2==s) << std::endl;//字符串的比较：0代表不相等，1代表相等
```

字符串的输入

```
    std::string s;
    std::cin >> s;//不用判断字符串大小
```

