# PHP高级

## 多维数组

```php
<?php
$cars = array(
    array(1, 2, 3),
    array(4, 5, 6),
    array(7, 8, 9, 0)
);

echo $cars[2][3];
```

## 文件操作

### 逐行读取

```php
<?php
$file = fopen("text.txt", "r");//打开文件

while (!feof($file)) {
    echo fgets($file) . "<br>";
}

fclose($file);//关闭文件
```

### 逐字符读取

```php
<?php
$file = fopen("text.txt", "r");//打开文件

while (!feof($file)) {
    echo fgetc($file) . "<br>";
}

fclose($file);//关闭文件
```

### 文件写入

```php
<?php
$file = fopen("text.txt", "w");//打开文件

fwrite($file,"测试Test");

fclose($file);//关闭文件
```

## 序列化

### 概念

​		序列化是将数据转换为一种字符串格式，以便于存储或传输，同时也能够在需要时重新恢复成原始数据的过程。
​		在 PHP 中，可以使用 `serialize()` 函数将对象、数组或其他数据结构序列化为字符串。相应地，可以使用 `unserialize()` 函数将序列化后的字符串转换回原始的数据结构或对象。序列化在数据持久化、数据传输以及在分布式系统中共享数据时非常有用。

### 例子

#### 未声明变量类型序列化

```php
<?php

class dog
{
    public $name = "sky";
    public $age = 18;
    public $weight = 73.5;
}

$myDog = new dog();

print(serialize($myDog));
```

```
O:3:"dog":3:{s:4:"name";s:3:"sky";s:3:"age";i:18;s:6:"weight";d:73.5;}
```

- `O` 表示这是一个对象；
- `3` 表示类名 `dog` 有 3 个字符；
- `"dog"` 是类名；
- `3` 表示这个对象有 3 个属性；
- `s:4:"name";`, `s:3:"age";`, `s:6:"weight";` 表示了类的属性名和长度，其中 `s:X` 表示属性名为字符串，后面的数字 `X` 是属性名长度；
- `s:3:"sky";` 表示字符串属性 `$name` 的值是 `"sky"`，长度为 3；
- `i:18;` 表示整数属性 `$age` 的值是 `18`；
- `d:73.5;` 表示浮点数属性 `$weight` 的值是 `73.5`。

#### 未赋初值序列化

当设置了具体类型(php7.4)的变量未附初始值序列化后将当做空变量处理(不显示)。

```php
<?php

class dog
{
    public string $name; //这里设置了类型 ！！！
    public $age;
    public $weight;
}

$myDog = new dog();

print(serialize($myDog));
```

```
O:3:"dog":2:{s:3:"age";N;s:6:"weight";N;}
```

