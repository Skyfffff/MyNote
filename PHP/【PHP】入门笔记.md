# PHP入门

## 输出

这些 PHP 输出函数在功能和用法上有一些区别：

1. **echo**:
   - 用于输出一个或多个字符串。
   - 不返回值，直接输出内容到屏幕。
   - 速度较快，适合简单的输出。
   
   ```php
   $str = "Hello, world!";
   echo $str;
   ```

2. **print**:
   - 用于输出一个字符串。
   - 返回值为 1，总是输出 1。
   - 输出会在语句执行完成后。

   ```php
   $str = "Hello, world!";
   print $str;
   ```

3. **printf, sprintf**:
   - `printf` 用于格式化输出字符串，直接将格式化后的字符串输出到屏幕。
   - `sprintf` 返回格式化后的字符串，但不直接输出到屏幕。

   ```php
   $num = 10;
   printf("The number is %d", $num); // 输出：The number is 10
   
   $formatted = sprintf("The number is %d", $num);
   echo $formatted; // 输出：The number is 10
   ```

4. **var_dump**:
   - 用于打印变量的详细信息，包括类型和值。
   - 不仅输出变量的值，还包括类型和长度等信息。

   ```php
   $arr = [1, 2, 3];
   var_dump($arr); // 输出数组的类型、长度和元素值
   ```

5. **print_r**:
   - 用于输出易读的变量信息，主要用于数组。
   - 输出数组内容，但不显示数据类型或长度等详细信息。

   ```php
   $arr = [1, 2, 3];
   print_r($arr); // 输出数组的元素值
   ```

6. **die, exit**:
   - 两者都用于终止脚本的执行。
   - `die` 和 `exit` 输出一条消息，然后停止脚本的执行。

   ```php
   $error_msg = "Something went wrong!";
   die($error_msg); // 输出错误消息并停止脚本执行
   ```

7. **header**:
   - 用于发送原始的 HTTP 头。
   - 主要用于控制页面的重定向和缓存等。

   ```php
   header("Location: https://www.example.com"); // 重定向到另一个页面
   ```

8. **ob_start, ob_get_clean**:
   - `ob_start` 开始输出缓冲区。
   - `ob_get_clean` 获取当前缓冲区的内容并且清除缓冲区。

   ```php
   ob_start(); // 开始输出缓冲区
   echo "Hello, ";
   $content = ob_get_clean(); // 获取缓冲区内容并清除缓冲区
   echo $content; // 输出：Hello,
   ```

这些函数各自有不同的特性和适用场景，可以根据需要选择最合适的输出函数来达到预期的输出效果。

## 变量声明

```php
<?php
$x = 5;
$y = 6;
$z = $x + $y;
echo $z;
```

## 常量声明

1. PHP 5.3之前：

   - 在PHP 5.3之前，常量的定义通常使用`define`函数进行，如下所示：

     ```php
     define("MY_CONSTANT", "This is a constant");
     ```

   - 常量的名称通常是区分大小写的。

2. PHP 5.3及以后：

   - PHP 5.3引入了更方便的常量定义方式，使用`const`关键字，如下所示：

     ```php
     const MY_CONSTANT = "This is a constant";
     ```

   - 使用`const`关键字定义的常量是类内部的类常量，也可以在全局范围内定义。

## EOF定义字符串

EOF代表"End of File"，它通常用于标识一个文本块或多行字符串的结束。EOF在PHP中用于创建多行字符串，它提供了一种在字符串中包含换行符和引号而不需要使用转义字符的方式。

```php
<?php
$str1 = "World";
$str2 = <<<EOF
        Hello $str1 !!!
EOF;
// 结束需要独立一行且前后不能空格
echo $str2;
```

- 必须后接分号，否则编译通不过。
- EOF可以用任意其它字符代替，只需保证结束标识与开始标识一致。
- **结束标识必须顶格独自占一行(即必须从行首开始，前后不能衔接任何空白和字符)。**
- 开始标识可以不带引号或带单双引号，不带引号与带双引号效果一致，解释内嵌的变量和转义符号，带单引号则不解释内嵌的变量和转义符号。
- 当内容需要内嵌引号（单引号或双引号）时，不需要加转义符，本身对单双引号转义，此处相当与q和qq的用法。

## 基本数据类型

### String（字符串）

```php
<?php
$str = "Hello World";
echo $str;
```

### Integer（整型）

### Float（浮点型）

```php
<?php
$int = 1;
$int2 = -1;
$int3 = 1.666;
$int4 = 0x66;
$int5 = 066;
echo $int, "<br>", $int2, "<br>", $int3, "<br>", $int4, "<br>", $int5;
```

### Boolean（布尔型）

```php
<?php
$bool = true;
$bool2 = false;
```

### Array（数组）

```php
<?php
$arr = array("xiaoshu","wenhan","shuge");
echo $arr[1];
var_dump($arr);
```

### Object（对象）

类属性必须定义为公有，受保护，私有之一。如果用 var 定义，则被视为公有。

```php
<?php

class Student
{
    private string $name;
    private int $sex;
    private int $age;
    

    /**
     * @return string
     */
    public function getName(): string
    {
        return $this->name;
    }

    /**
     * @param string $name
     */
    public function setName(string $name): void
    {
        $this->name = $name;
    }

    /**
     * @return int
     */
    public function getSex(): int
    {
        return $this->sex;
    }

    /**
     * @param int $sex
     */
    public function setSex(int $sex): void
    {
        $this->sex = $sex;
    }

    /**
     * @return int
     */
    public function getAge(): int
    {
        return $this->age;
    }

    /**
     * @param int $age
     */
    public function setAge(int $age): void
    {
        $this->age = $age;
    }
}

$student = new Student();
$student->setAge(21);
echo $student->getAge();
```

### NULL（空值）

```php
<?php
$x = null;
var_dump($x);
```

### Resource（资源类型）

```php
<?php
$fp = fopen("foo","w");
echo get_resource_type($fp)."\n";
// 打印：file
```

PHP 资源 resource 是一种特殊变量，保存了到外部资源的一个引用。

常见资源数据类型有打开文件、数据库连接、图形画布区域等。

由于资源类型变量保存有为打开文件、数据库连接、图形画布区域等的特殊句柄，因此将其它类型的值转换为资源没有意义。

使用 **get_resource_type()** 函数可以返回资源（resource）类型：

```
get_resource_type(resource $handle): string
```

此函数返回一个字符串，用于表示传递给它的 resource 的类型。如果参数不是合法的 resource，将产生错误。

## 运算符

### 基本运算符

| 运算符 | 名称             | 描述                                                         | 实例                         | 结果  |
| :----- | :--------------- | :----------------------------------------------------------- | :--------------------------- | :---- |
| x + y  | 加               | x 和 y 的和                                                  | 2 + 2                        | 4     |
| x - y  | 减               | x 和 y 的差                                                  | 5 - 2                        | 3     |
| x * y  | 乘               | x 和 y 的积                                                  | 5 * 2                        | 10    |
| x / y  | 除               | x 和 y 的商                                                  | 15 / 5                       | 3     |
| x % y  | 模（除法的余数） | x 除以 y 的余数                                              | 5 % 2 10 % 8 10 % 2          | 1 2 0 |
| -x     | 设置负数         | 取 x 的相反符号                                              | `<?php $x = 2; echo -$x; ?>` | -2    |
| ~x     | 取反             | x 取反，按二进制位进行"取反"运算。运算规则：`~1=-2;    ~0=-1;` | `<?php $x = 2; echo ~$x; ?>` | -3    |
| a . b  | 并置             | 连接两个字符串                                               | "Hi" . "Ha"                  | HiHa  |

### 比较运算符

- 松散比较：使用两个等号 **==** 比较，只比较值，不比较类型。
- 严格比较：用三个等号 **===** 比较，除了比较值，也比较类型。

```php
<?php
if (42 == "42") {
    echo '1、值相等';
}

echo PHP_EOL; // 换行符

if (42 === "42") {
    echo '2、类型相等';
} else {
    echo '3、类型不相等';
}
```

## 数组

### 普通数组

#### 声明

```php
<?php
$cars = array("Volvo", 111, "Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
```

#### 遍历

##### foreach遍历

```php
<?php
$arrs = array("Volvo", 111, "Toyota");
foreach ($arrs as $arr) {
    echo $arr;
}
```

##### for循环遍历

```php
<?php
$arrs = array("Volvo", 111, "Toyota");
$length = count($arrs);

for ($i = 0; $i < $length; $i++) {
    echo $arrs[$i];
}
```

### 关联数组(map)

#### 定义

```php
<?php
$arrs = array("name" => "Sky", "sex" => 1, "age" => 18);
```

#### 遍历

```php
<?php
$arrs = array("name" => "Sky", "sex" => 1, "age" => 18);
foreach ($arrs as $key => $value) {
    echo "$key:$value<br>";
}
```

## 命名空间

在PHP中，`namespace`用于将一个类、函数或常量放置在一个命名空间中，以避免命名冲突和组织代码。以下是`namespace`的基本用法：

1. **定义命名空间**：你可以在PHP文件的顶部使用`namespace`关键字来定义一个命名空间。通常，你应该在`namespace`声明之前放置任何代码。

```php
namespace MyNamespace;
```

2. **使用命名空间**：一旦定义了命名空间，你可以在同一个文件或其他文件中引入并使用它。使用`use`关键字引入命名空间，并在代码中实例化类、调用函数或使用常量时指定命名空间。

```php
use MyNamespace\MyClass;

$object = new MyClass();
```

3. **绝对命名空间和相对命名空间**：当引入一个类或函数时，你可以使用绝对命名空间或相对命名空间。绝对命名空间以反斜杠`\`开头，从全局命名空间开始，而相对命名空间没有反斜杠`\`，它们相对于当前命名空间。

```php
use MyNamespace\AnotherNamespace\AnotherClass; // 绝对命名空间

use .\AnotherNamespace\AnotherClass; // 相对命名空间
```

4. **多个命名空间**：你可以在一个文件中定义多个命名空间，但不建议这么做，因为通常一个文件应该只包含一个命名空间。

```php
namespace MyNamespace;
// 一些代码

namespace AnotherNamespace;
// 一些代码
```

5. **全局命名空间**：如果你没有在代码中定义命名空间，那么它将属于全局命名空间，这是PHP的默认命名空间。

这是一个简单的示例，展示了如何在PHP中使用`namespace`：

```php
namespace MyNamespace;

class MyClass {
    public function sayHello() {
        echo 'Hello from MyClass!';
    }
}

// 在其他文件或相同文件的不同命名空间中使用
use MyNamespace\MyClass;

$object = new MyClass();
$object->sayHello();
```

通过使用命名空间，你可以更好地组织和管理你的PHP代码，减少命名冲突，并提高代码的可维护性。

## 接口

PHP 接口（Interface）是一种定义了一组方法但没有实现方法体的结构。它们用于定义一组方法，这些方法由实现该接口的类来实现。接口在 PHP 中有很多用途，最常见的是用于定义规范，以确保类遵循一定的方法签名和结构。

以下是 PHP 接口的基本用法和语法：

1. 声明接口：
   使用 `interface` 关键字来声明一个接口。接口中的方法没有方法体（即没有实际的实现代码），只有方法签名。

   ```php
   interface MyInterface {
       public function method1();
       public function method2($param);
   }
   ```

2. 实现接口：
   类使用 `implements` 关键字来实现接口，并必须实现接口中定义的所有方法。接口的方法签名必须在实现类中完全匹配，包括参数名和顺序。

   ```php
   class MyClass implements MyInterface {
       public function method1() {
           // 实现 method1
       }
   
       public function method2($param) {
           // 实现 method2
       }
   }
   ```

3. 多接口实现：
   一个类可以实现多个接口，使用逗号分隔接口名称。

   ```php
   class MyAnotherClass implements MyInterface, AnotherInterface {
       // 实现接口方法
   }
   ```

4. 访问接口常量：
   接口也可以包含常量，并且可以使用类似 `$interfaceName::CONSTANT_NAME` 的方式访问。

   ```php
   interface MyInterface {
       const MY_CONSTANT = 42;
   }
   
   echo MyInterface::MY_CONSTANT; // 输出 42
   ```

5. 接口继承：
   接口可以继承其他接口，允许在一个接口中定义另一个接口的方法。实现类需要实现继承接口中定义的所有方法。

   ```php
   interface ParentInterface {
       public function parentMethod();
   }
   
   interface ChildInterface extends ParentInterface {
       public function childMethod();
   }
   ```

注意：接口不允许包含属性（成员变量），只能包含方法签名。方法在实现类中需要被定义并提供实际的实现代码。

使用接口可以帮助你在 PHP 中实现抽象的、可复用的代码结构，同时确保实现类遵守规范。

## Static关键字

在PHP中，`static`是一个关键字，通常用于类和方法中，具有不同的含义和用途。

1. **静态属性（Static Properties）：** 在类中使用`static`关键字可以创建静态属性。静态属性属于类，而不是类的实例。这意味着无论创建了多少个类的实例，静态属性只有一份，所有实例都共享它。例如：

```php
class MyClass {
    public static $myStaticVar = 0;
}

MyClass::$myStaticVar = 5; // 访问静态属性并设置其值
```

2. **静态方法（Static Methods）：** 使用`static`关键字可以创建静态方法。静态方法可以通过类本身而不是类的实例来调用。这些方法通常用于实用程序功能，而不需要与特定实例交互。例如：

```php
class MathUtility {
    public static function add($a, $b) {
        return $a + $b;
    }
}

$result = MathUtility::add(5, 3); // 调用静态方法
```

3. **静态变量（Static Variables）：** 在函数内部，使用`static`关键字可以创建静态变量。静态变量在函数调用之间保持其值，不像普通局部变量，每次函数调用时都会重新初始化。这可用于在函数调用之间维护状态信息。

```php
function countCalls() {
    static $count = 0;
    $count++;
    echo "Function called $count times.<br>";
}

countCalls();
countCalls();
```

总之，`static`关键字用于创建静态属性、静态方法和静态变量，它们在类和函数中具有不同的用途，但都与保持状态信息或在不同上下文中共享信息有关。

## Final关键字

在PHP中，`final`是一个关键字，用于标记类、方法和属性，具有不同的含义和用途：

1. **Final Class（最终类）**：当你将`final`关键字用于一个类时，这个类将被标记为最终类。最终类不能被其他类继承，也就是说它是不可扩展的。这通常用于阻止其他开发人员继承某个类，以确保它的行为不会被修改或扩展。

   ```php
   final class MyFinalClass {
       // Class definition
   }
   ```

2. **Final Method（最终方法）**：在方法前面使用`final`关键字可以标记该方法为最终方法。最终方法不能被子类重写或覆盖。这对于确保某些类的核心行为不被修改非常有用。

   ```php
   class MyClass {
       final public function myFinalMethod() {
           // Method definition
       }
   }
   ```

3. **Final Property（最终属性）**：在PHP中，没有与属性相关的`final`关键字。属性的最终性（不可更改）通常通过将属性声明为`private`（私有）并提供公共访问器和设置器来实现，以确保只有类的内部可以访问和修改属性的值。

总之，`final`关键字用于限制类、方法和属性的扩展或修改。当你将类或方法标记为最终时，它们不能被继承或重写，从而确保它们的行为不会被改变。这可以有助于在编写库或框架时确保核心功能的稳定性和一致性。