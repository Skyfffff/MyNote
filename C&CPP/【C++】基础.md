# C++基础

## 名词解释

### C++主流编译器

1. MSVC（Microsoft Visual C++）：是微软 Visual Studio IDE 的默认 C++ 编译器，只能在 Windows 平台使用。
2. GCC：GNU Compiler Collection 的缩写，是一个跨平台的编译器套件，支持多种编程语言，包括 C++。
3. Clang：是一个 C++ 编译器，由苹果公司开发，支持多种平台，包括 macOS、Linux 和 Windows。
4. Intel C++ Compiler：是由英特尔公司开发的 C++ 编译器，支持多种平台，包括 Windows、Linux 和 macOS。
5. Borland C++：是 Borland 公司开发的 C++ 编译器，现在已经被 Embarcadero 公司继承和发展，只能在 Windows 平台使用。
6. MinGW：是一个在 Windows 平台下编译使用 GNU 工具集的开发环境，其中包含了 GCC 编译器。
7. Clion: 是 JetBrains 公司开发的 C++ IDE，它集成了 CMake，并支持多种编译器，如 GCC、Clang、MSVC 等。

### C++主流IDE

1. Microsoft Visual Studio：是微软公司开发的集成开发环境，支持多种编程语言，包括 C++，只能在 Windows 平台使用。
2. Xcode：是苹果公司开发的集成开发环境，支持多种编程语言，包括 C++，只能在 macOS 平台使用。
3. Eclipse：是一个跨平台的开发环境，支持多种编程语言，包括 C++。
4. NetBeans：是一个跨平台的开发环境，支持多种编程语言，包括 C++。
5. CLion：是 JetBrains 公司开发的 C++ 集成开发环境，集成了 CMake，支持多种编译器，如 GCC、Clang、MSVC 等。
6. Qt Creator：是 Qt 开发工具的官方 IDE，支持多种编程语言，包括 C++，集成了 Qt 库和工具。
7. Code::Blocks：是一个开源的跨平台集成开发环境，支持多种编程语言，包括 C++。
8. Dev-C++：是一个免费的 C++ 集成开发环境，支持多种编译器，如 GCC、MinGW 等。

## 数据类型

### 基本数据类型

> C++ 为程序员提供了种类丰富的内置数据类型和用户自定义的数据类型。下表列出了七种基本的 C++ 数据类型：

| 类型     | 关键字  |
| :------- | :------ |
| 布尔型   | bool    |
| 字符型   | char    |
| 整型     | int     |
| 浮点型   | float   |
| 双浮点型 | double  |
| 无类型   | void    |
| 宽字符型 | wchar_t |

#### 类型及其范围

| 类型               | 位            | 范围                                                         |
| :----------------- | :------------ | :----------------------------------------------------------- |
| char               | 1 个字节      | -128 到 127 或者 0 到 255                                    |
| unsigned char      | 1 个字节      | 0 到 255                                                     |
| signed char        | 1 个字节      | -128 到 127                                                  |
| int                | 4 个字节      | -2147483648 到 2147483647                                    |
| unsigned int       | 4 个字节      | 0 到 4294967295                                              |
| signed int         | 4 个字节      | -2147483648 到 2147483647                                    |
| short int          | 2 个字节      | -32768 到 32767                                              |
| unsigned short int | 2 个字节      | 0 到 65,535                                                  |
| signed short int   | 2 个字节      | -32768 到 32767                                              |
| long int           | 8 个字节      | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807      |
| signed long int    | 8 个字节      | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807      |
| unsigned long int  | 8 个字节      | 0 到 18,446,744,073,709,551,615                              |
| float              | 4 个字节      | 精度型占4个字节（32位）内存空间，+/- 3.4e +/- 38 (~7 个数字) |
| double             | 8 个字节      | 双精度型占8 个字节（64位）内存空间，+/- 1.7e +/- 308 (~15 个数字) |
| long double        | 16 个字节     | 长双精度型 16 个字节（128位）内存空间，可提供18-19位有效数字。 |
| wchar_t            | 2 或 4 个字节 | 1 个宽字符                                                   |

#### 不同环境占用字节数

<img src="PictureFile/【C++】基础.assets/32-64.jpg" alt="img" style="zoom:50%;" />

> 查看本系统占用情况

```c++
#include<iostream>
#include <limits>

using namespace std;

int main()
{
    cout << "type: \t\t" << "************size**************"<< endl;
    cout << "bool: \t\t" << "所占字节数：" << sizeof(bool);
    cout << "\t最大值：" << (numeric_limits<bool>::max)();
    cout << "\t\t最小值：" << (numeric_limits<bool>::min)() << endl;
    cout << "char: \t\t" << "所占字节数：" << sizeof(char);
    cout << "\t最大值：" << (numeric_limits<char>::max)();
    cout << "\t\t最小值：" << (numeric_limits<char>::min)() << endl;
    cout << "signed char: \t" << "所占字节数：" << sizeof(signed char);
    cout << "\t最大值：" << (numeric_limits<signed char>::max)();
    cout << "\t\t最小值：" << (numeric_limits<signed char>::min)() << endl;
    cout << "unsigned char: \t" << "所占字节数：" << sizeof(unsigned char);
    cout << "\t最大值：" << (numeric_limits<unsigned char>::max)();
    cout << "\t\t最小值：" << (numeric_limits<unsigned char>::min)() << endl;
    cout << "wchar_t: \t" << "所占字节数：" << sizeof(wchar_t);
    cout << "\t最大值：" << (numeric_limits<wchar_t>::max)();
    cout << "\t\t最小值：" << (numeric_limits<wchar_t>::min)() << endl;
    cout << "short: \t\t" << "所占字节数：" << sizeof(short);
    cout << "\t最大值：" << (numeric_limits<short>::max)();
    cout << "\t\t最小值：" << (numeric_limits<short>::min)() << endl;
    cout << "int: \t\t" << "所占字节数：" << sizeof(int);
    cout << "\t最大值：" << (numeric_limits<int>::max)();
    cout << "\t最小值：" << (numeric_limits<int>::min)() << endl;
    cout << "unsigned: \t" << "所占字节数：" << sizeof(unsigned);
    cout << "\t最大值：" << (numeric_limits<unsigned>::max)();
    cout << "\t最小值：" << (numeric_limits<unsigned>::min)() << endl;
    cout << "long: \t\t" << "所占字节数：" << sizeof(long);
    cout << "\t最大值：" << (numeric_limits<long>::max)();
    cout << "\t最小值：" << (numeric_limits<long>::min)() << endl;
    cout << "unsigned long: \t" << "所占字节数：" << sizeof(unsigned long);
    cout << "\t最大值：" << (numeric_limits<unsigned long>::max)();
    cout << "\t最小值：" << (numeric_limits<unsigned long>::min)() << endl;
    cout << "double: \t" << "所占字节数：" << sizeof(double);
    cout << "\t最大值：" << (numeric_limits<double>::max)();
    cout << "\t最小值：" << (numeric_limits<double>::min)() << endl;
    cout << "long double: \t" << "所占字节数：" << sizeof(long double);
    cout << "\t最大值：" << (numeric_limits<long double>::max)();
    cout << "\t最小值：" << (numeric_limits<long double>::min)() << endl;
    cout << "float: \t\t" << "所占字节数：" << sizeof(float);
    cout << "\t最大值：" << (numeric_limits<float>::max)();
    cout << "\t最小值：" << (numeric_limits<float>::min)() << endl;
    cout << "size_t: \t" << "所占字节数：" << sizeof(size_t);
    cout << "\t最大值：" << (numeric_limits<size_t>::max)();
    cout << "\t最小值：" << (numeric_limits<size_t>::min)() << endl;
    cout << "string: \t" << "所占字节数：" << sizeof(string) << endl;
    cout << "type: \t\t" << "************size**************"<< endl;
    return 0;
}
```

```c++
type:           ************size**************
bool:           所占字节数：1   最大值：1               最小值：0
char:           所占字节数：1   最大值：127             最小值：€
signed char:    所占字节数：1   最大值：127             最小值：€
unsigned char:  所占字节数：1   最大值：255             最小值：
wchar_t:        所占字节数：2   最大值：65535           最小值：0
short:          所占字节数：2   最大值：32767           最小值：-32768
int:            所占字节数：4   最大值：2147483647      最小值：-2147483648
unsigned:       所占字节数：4   最大值：4294967295      最小值：0
long:           所占字节数：4   最大值：2147483647      最小值：-2147483648
unsigned long:  所占字节数：4   最大值：4294967295      最小值：0
double:         所占字节数：8   最大值：1.79769e+308    最小值：2.22507e-308
long double:    所占字节数：16  最大值：1.18973e+4932   最小值：3.3621e-4932
float:          所占字节数：4   最大值：3.40282e+38     最小值：1.17549e-38
size_t:         所占字节数：8   最大值：18446744073709551615    最小值：0
string:         所占字节数：32
type:           ************size**************
```

### 常量

#### 整数常量

> 整数常量也可以带一个后缀，后缀是 U 和 L 的组合，U 表示无符号整数（unsigned），L 表示长整数（long）。后缀可以是大写，也可以是小写，U 和 L 的顺序任意。

```c++
#include <iostream>
#include <bitset>

using namespace std;

int main() {
    cout << 0b111 << endl; //二进制
    cout << 0111 << endl; //八进制
    cout << 111 << endl; //十进制
    cout << 0x111 << endl; //十六进制

    //二进制输出 输出位数为16
    cout << bitset<16>(111) << endl;
    //八进制输出
    cout << oct << 111 << endl;
    //十进制输出（默认）
    cout << 111 << endl;
    //十六进制输出
    cout << hex << 111 << endl;
}
```

#### 浮点常量

> 浮点常量由整数部分、小数点、小数部分和指数部分组成。您可以使用小数形式或者指数形式来表示浮点常量。
>
> 当使用小数形式表示时，必须包含整数部分、小数部分，或同时包含两者。当使用指数形式表示时， 必须包含小数点、指数，或同时包含两者。带符号的指数是用 e 或 E 引入的。

```c++
3.14159       // 合法的 
314159E-5L    // 合法的 
510E          // 非法的：不完整的指数
210f          // 非法的：没有小数或指数
.e55          // 非法的：缺少整数或分数
```

#### 布尔常量

布尔常量共有两个，它们都是标准的 C++ 关键字：

- **true** 值代表真。
- **false** 值代表假。

我们不应把 true 的值看成 1，把 false 的值看成 0。

#### 字符常量

> 字符常量是括在单引号中。如果常量以 L（仅当大写时）开头，则表示它是一个宽字符常量（例如 L'x'），此时它必须存储在 **wchar_t** 类型的变量中。否则，它就是一个窄字符常量（例如 'x'），此时它可以存储在 **char** 类型的简单变量中。
>
> 字符常量可以是一个普通的字符（例如 'x'）、一个转义序列（例如 '\t'），或一个通用的字符（例如 '\u02C0'）。
>
> 在 C++ 中，有一些特定的字符，当它们前面有反斜杠时，它们就具有特殊的含义，被用来表示如换行符（\n）或制表符（\t）等。

| 转义序列   | 含义                       |
| :--------- | :------------------------- |
| \\         | \ 字符                     |
| \'         | ' 字符                     |
| \"         | " 字符                     |
| \?         | ? 字符                     |
| \a         | 警报铃声                   |
| \b         | 退格键                     |
| \f         | 换页符                     |
| \n         | 换行符                     |
| \r         | 回车                       |
| \t         | 水平制表符                 |
| \v         | 垂直制表符                 |
| \ooo       | 一到三位的八进制数         |
| \xhh . . . | 一个或多个数字的十六进制数 |

#### 字符串常量

> 字符串字面值或常量是括在双引号 **""** 中的。一个字符串包含类似于字符常量的字符：普通的字符、转义序列和通用的字符。
>
> 您可以使用 **\** 做分隔符，把一个很长的字符串常量进行分行。

#### 定义常量

在 C++ 中，有两种简单的定义常量的方式：

- 使用 **#define** 预处理器。
- 使用 **const** 关键字。

```c++
#include<iostream>

using namespace std;
#define PI 3.14;

int main() {
    cout << PI;
}
```

```c++
#include<iostream>

using namespace std;

int main() {
    const double PI = 3.14;
    cout << PI;
}
```

> 常量内存空间

```c++
void test07() {
    const int i = 100;
    //默认值 未开辟内存空间
    cout << i << endl;
    //取地址后开辟内存空间
    int *p = (int *) &i;
    *p = 2000;
    //值发生变化
    cout << *p << endl;
    cout << i << endl;
}
```

### 枚举类型

```c++
enum 枚举名{ 
     标识符[=整型常数], 
     标识符[=整型常数], 
... 
    标识符[=整型常数]
} 枚举变量;
```

```c++
enum color{
    red,
    blue,
    yellow
}c;

using namespace std;
int main(){
    c = red;
    cout << c;
    c = blue;
    cout << c;
}
```

### 类型转换

#### 强制类型转换

```c++
void test09(){
    float i = 10.56;
    cout << (int)i;
}
```

#### 静态类型转换（Static Cast）

> 静态转换是将一种数据类型的值强制转换为另一种数据类型的值。
>
> 静态转换通常用于比较类型相似的对象之间的转换，例如将 int 类型转换为 float 类型。
>
> 静态转换不进行任何运行时类型检查，因此可能会导致运行时错误。

```c++
#include<iostream>

using namespace std;

int main() {
    int i = 128;
    auto f = static_cast<char>(i);
    ::printf("%d", f);
}
```

#### 动态类型转换（Dynamic Cast）

> 动态转换通常用于将一个基类指针或引用转换为派生类指针或引用。动态转换在运行时进行类型检查，如果不能进行转换则返回空指针或引发异常。

#### 常量转换（Const Cast）

> 常量转换用于将 const 类型的对象转换为非 const 类型的对象。
>
> 常量转换只能用于转换掉 const 属性，不能改变对象的类型。

#### 重新解释转换（Reinterpret Cast）

> 重新解释转换将一个数据类型的值重新解释为另一个数据类型的值，通常用于在不同的数据类型之间进行转换。
>
> 重新解释转换不进行任何类型检查，因此可能会导致未定义的行为。

## 数组

### 定义

```c++
//一维
int arr1[5];
//二维
int arr2[2][2];
//函数数组 每个函数为函数入口地址 返回值int 传参两个int
int (*arr3[5])(int, int);
```

### 初始化

> 未初始化内容随机

```c++
void test11() {
    int x = 6;
    int arr[x];
    cout << sizeof(arr) << endl;
    for (int i = 0; i < 6; ++i) {
        cout << &arr[i] << endl;
    }
}
```

> 初始化

```c++
//全部初始化为0
int arr1[5] = {0};
//二维
int arr2[2][2] = {0};

for (int i : arr1) {
    cout << i << endl;
}

for (const auto &item: arr2){
    for (const auto i: item){
        cout << i << endl;
    }
}
```

## 预处理

### 内存分区

<img src="PictureFile/【C++】基础.assets/image-20230313195657621.png" alt="image-20230313195657621" style="zoom:50%;" />

### 头文件包含

```c++
#include <iostream> //从系统目录找
#include "head.h" //先从当前目录找 再从系统目录找
```

## 指针

### 定义

> 本机所有指针大小为8字节

```c++
double (*p)[3][3];
cout << sizeof(int *) << endl;
cout << sizeof(float *) << endl;
cout << sizeof(long *) << endl;
cout << sizeof(long int *) << endl;
cout << sizeof(*p) << endl;
```

### 指针问题

> 未被初始化的指针指向哪里？
>
> 答：在大多数情况下，当一个指针没有被初始化或者被初始化为0时，它将具有空指针的值，即0（或NULL）。需要注意的是，具体的指针地址可能会因为操作系统、编译器、计算机硬件等因素而有所不同。因此，如果您在不同的计算机或编译器上运行程序，可能会得到不同的指针地址。

```c++
int *p;
cout << p << endl; //输出内容不确定或为0

int *q = nullptr;
cout << q << endl;//输出内容为0
```

## 动态内存

### new与delete

```c++
//内存分配
int *p;
p = new int;
cout << *p << endl;
delete p;

//内存分配并且初始化
char *q;
q = new char(65);
cout << *q << endl;
delete q;

//数组分配
int *p1 = new int[5];
delete[] p1;
```

## 字符串

> 需要头文件 string.h

```c++
void test24() {
    char str1[100] = "hello";
    char str2[] = "world";
    //字符串长度
    cout << strlen(str1) << endl;
    cout << strlen(str2) << endl;

    //复制字符串
    cout << strcpy(str1, str2) << endl;
    cout << str1 << endl;
    cout << str2 << endl;

    //复制字符串(按照长度)
    cout << strncpy(str1, str2, 2) << endl;
    cout << str1 << endl; //被替换前两个字符
    cout << str2 << endl; //不会变

    //字符串追加
    cout << strcat(str1, "sky") << endl;
    cout << str1 << endl; //被追加sky
    cout << str2 << endl; //不会变

    //字符串比较(大于零str1大于str2 以此类推)
    cout << strcmp(str1, str2) << endl;
}
```

```c++
5
5
world
world
world
world
world
world
worldsky
worldsky
world
1
```

## 结构体

### 定义

```c++
struct Student {
    int sno;
    char name[50];
};
/*
在 C++ 中，未初始化的变量可以具有任意值。在您的代码中，student3 对象是未初始化的，这意味着在内存中分配了一些随机值。然后，您使用 cin 对其进行了输入操作，将输入的值存储到 student3.name 和 student3.sno 中。

尽管 student3 是未初始化的，但在使用 cin 对其进行输入时，您实际上是将输入的值存储到 student3.name 和 student3.sno 占据的内存位置。因此，您可以成功地将输入值存储在未初始化的对象中。

但是，由于 student3 是未初始化的，因此不能保证在其内存位置中没有其他数据。因此，在使用 cout 输出 student3.name 和 student3.sno 时，您可能会看到奇怪的字符或不可预测的结果，因为 name 字符数组中的未定义值可以包含任何值，甚至包括空字符，这会影响在 cout 中输出 name 字符串时的结果。

因此，建议在使用变量之前始终对其进行初始化，以避免出现未定义的行为。
*/
void test25(){
    //初始化为空值
    Student student3 = {};
    //声明并且初始化
    Student student2 = {2103130131,"sky"};
    cout << student2.sno << " " << student2.name[0] << endl;
    //未初始化
    Student student;
    cout << student.name << "-" << student.sno << endl;
}
```

```
2103130131 sky
-0
```

### 结构体数组

```c++
void test26(){
    //结构体数组
    Student students[] = {{1,"sky"},{2,"shabo"},{3,"wenhan"}};
    int len = sizeof(students) / sizeof(students[0]);
    for (int i = 0; i <len; ++i) {
        cout << students[i].sno << "--" << students[i].name << endl;
    }
}
```

```
1--sky
2--shabo
3--wenhan
```

### 内存对齐规则

1. **第一个成员在结构体变量偏移位的0地址处。**
2. **其他变量在偏移自身大小整数倍处。**
3. **结构体总大小为最大对齐数（vs默认为8）的整数倍**

```c++
struct book {
    int id;
    char name;
    char x;
    char x2;
    char x3;
    char x4;
};
//结构体大小为12
```

```c++
struct Student {
    int sno;
    char name[50];
};
//结构体大小为56
```

## 共用体

```c++
union u{
    int a;
    char b;
};
```

```c++
int main() {
    u u1 = {};
    u1.a = 1;
    u1.b = '1';
    cout << sizeof(u) << endl;
    cout << u1.a << endl;
    cout << u1.b << endl;
}
```

```
4
49
1
```

## 枚举

```c++
enum e{
    a,
    b,
    c
};
```

```
0
1
2
```

## 命名空间

```c++
namespace A{
    int a =1;
    char b = 'A';
    int arr[5] = {1,2,3,4,5};
    int func(int x) { 
        return x + 1;
    }
}

void test27() {
    cout << A::a << endl;
    cout << A::b << endl;
    cout << A::arr << endl;
    cout << A::func(1) << endl;
}
```

```
1
A
0x7ff7ac137020
2
```

## 类与对象

### 类的创建

> Book.h

```c++
#ifndef HELLOWORLD_BOOK_H
#define HELLOWORLD_BOOK_H


class Book {
private:
    int bookId;
public:
    void setBookId(int b);

    int getBookId() const;
};


#endif //HELLOWORLD_BOOK_H
```

> Book.cpp

```c++
#include "Book.h"

void Book::setBookId(int b) {
    bookId = b;
}

int Book::getBookId() const {
    return bookId;
}
```

### 类的初始化

```c++
#include "header/Book.h"

int main() {
    Book book{};
    book.setBookId(123);
    cout << book.getBookId() << "";
}
```

### 构造器

#### 无参构造器

> 系统默认带无参构造器

```c++
class Data {
private:
    char *arr;
public:
    //构造函数
    Data() {
        arr = nullptr;
        cout << "无参构造器" << endl;
    }
};
int main() {
    //调用无参构造器
    Data data1;
}
```

#### 有参构造器

```c++
class Data {
private:
    char *arr;
public:
    Data(char *a) {
        arr = new char[::strlen(a) + 1];
        ::strcpy(arr, a);
        cout << "有参构造器" << endl;
    }
};
int main() {
    //有参构造器
    char arr[5] = {1, 2, 3, 4};
    Data data2(arr);
}
```

#### 析构函数

```c++
class Data {
private:
    char *arr;
public:
	//析构函数
    ~Data() {
        delete[] arr;
        cout << "释放" << endl;
    }
};
int main() {
    //无参构造器
    Data data1;
}
```

#### 拷贝构造函数

> 默认自带浅拷贝构造函数

```c++
class Data {
public:
    char *arr;
public:
    Data(const Data &d) {
        arr = d.arr;
    }
};

int main() {
    Data da("123");
    Data db = da;
    cout << db.arr[0] << endl;
}
```

> 深拷贝

```c++
class Data {
public:
    char *arr;
public:
    Data(const Data &d) {
        arr = new char[::strlen(d.arr) + 1];
        ::strcpy(arr, d.arr);
    }
};

int main() {
    Data da("123");
    Data db = da;
    cout << db.arr[0] << endl;
}
```

### 对象成员和初始化列表

```c++
class aClass {
private:
    int intA;
public:
    aClass() {
        cout << "aClass无参" << endl;
    }

    aClass(int a) {
        this->intA = a;
        cout << "aClass有参----" << a << endl;
    }

    ~aClass() {
        cout << "aClass释放" << endl;
    }
};

class bClass {
private:
    int intB;
    //对象成员
    aClass oa;
public:
    bClass() {
        cout << "bClass无参" << endl;
    }
	//初始化列表
    bClass(int b) : oa(b) {
        this->intB = b;
        cout << "bClass有参----" << b << endl;
    }

    ~bClass() {
        cout << "bClass释放" << endl;
    }
};

int main() {
    bClass b(1);
}
```

```
aClass有参----1
bClass有参----1
bClass释放
aClass释放
```

### 动态对象

### 单例模式

### 友元

### 智能指针

## 继承

```c++
class Father {
public:
    int a;
protected:
    int b;
private:
    int c;
};

class Son : public Father {
public:
    int d;
protected:
    int e;
private:
    int f;
};
```

## 多态

### 虚函数

```c++
class Animal{
public:
     virtual void func(){
        cout << "动物speak" << endl;
    }
};

class Dog:public Animal{
public:
    void func(){
        cout << "狗在叫" << endl;
    }
};

int main() {
    Animal *p = new Dog;
    p->func(); //狗在叫
}
```

### 纯虚函数

```c++
class Animal {
public:
    virtual void func() = 0;
};

class Dog : public Animal {
public:
    void func() {
        cout << "狗在叫" << endl;
    }
};

int main() {
    Animal *p = new Dog;
    p->func(); //狗在叫
}
```

### 虚析构函数

```c++
class Animal {
public:
    virtual void func() = 0;

    virtual ~Animal() { //虚析构
        cout << "Animal析构函数" << endl;
    }
};

class Dog : public Animal {
public:
    void func() {
        cout << "狗在叫" << endl;
    }

    ~Dog() {
        cout << "Dog析构函数" << endl;
    }
};

int main() {
    Animal *p = new Dog;
    p->func(); //狗在叫
    delete p;
}
```

```
狗在叫
Dog析构函数
Animal析构函数
```

### 纯虚析构函数

> 需要实现函数体

```c++
class Animal {
public:
    virtual void func() = 0;

    virtual ~Animal() = 0;
};

Animal::~Animal() {
    cout << "Animal析构函数" << endl;
}

class Dog : public Animal {
public:
    void func() override {
        cout << "狗在叫" << endl;
    }


    ~Dog() override {
        cout << "Dog析构函数" << endl;
    }
};

int main() {
    Animal *p = new Dog;
    p->func(); //狗在叫
    delete p;
}
```

## 模板

### 函数模板

#### 普通模板

```c++
template<typename T>
void Swap(T &a, T &b) {
    T temp = a;
    a = b;
    b = temp;
}

int main() {
    int a = 1, b = 2;
    Swap<>(a, b);
    cout << a << b << endl;
}
```

#### 函数模板具体化

### 类模板

```c++
template<class T1, class T2>
class myClass {
private:
    T1 a;
    T2 b;
public:
    myClass() = default;

    myClass(T1 mA, T2 mB);

    void printALL() {
        cout << a << b << endl;
    }
};

template<class T1, class T2>
myClass<T1, T2>::myClass(T1 mA, T2 mB) {
    a = mA;
    b = mB;
}


int main() {
    myClass m = myClass("sky", 666);
    m.printALL();
}
```

## 异常

```c++
void exceptionTest() {
    try {
        throw 1;
    } catch (int e) {
        cout << "有异常抛出:" << e << endl;
    } 
}

int main() {
    exceptionTest();
}
```

## STL容器

### string容器

#### string的存取

```c++
string str = "Sky678";
str[0] = 's'; // 无边界检查
str.at(1) = 's';// 有边界检查
cout << str << endl;
```

```
ssy678
```

#### string的查找和替换

```c++
void print(std::string::size_type n, std::string const &s)
{
    if (n == std::string::npos) {
        std::cout << "not found\n";
    } else {
        std::cout << "found: " << n << '\n';
    }
}

int main()
{
    std::string::size_type n;
    std::string const s = "This is a string";

    // 从头开始搜索
    n = s.find("is");
    print(n, s);

    // 从位置 5 开始搜索
    n = s.find("is", 5);
    print(n, s);

    // 寻找单个字符
    n = s.find('a');
    print(n, s);

    // 寻找单个字符
    n = s.find('q');
    print(n, s);
}
```

### vector容器

#### 容器声明

```c++
#include <vector>

int main() {
    vector<int> arr; //大小为0
    vector<int> arr1(10); //创建一个大小为10的容器
}
```

#### 容器大小

```c++
//获取容器大小
arr.size();
//获取容器容量
arr.capacity();
```

#### 添加元素

```c++
#include <vector>

int main() {
    vector<int> arr;
    //在元素末尾添加元素
    arr.push_back(123);
    //向数组第二个位置添加插入元素
    arr.insert(arr.begin() + 1, 9);
}
```

#### 删除元素

```c++
//删除第一个元素
arr.erase(arr.begin());
//删除最后一个元素
arr.pop_back();
```

#### 修改/访问元素

```c++
//修改第一个元素
arr[0] = 666;
arr.at(0) = 666; //有范围检查
```

#### 扩容容器

```c++
arr.reserve(1000); //修改
```

