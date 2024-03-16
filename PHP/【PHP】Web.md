# PHP Web

## 获取get OR post请求

> form.html
>

```html
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
 
<form action="welcome.php" method="post">
名字: <input type="text" name="fname">
年龄: <input type="text" name="age">
<input type="submit" value="提交">
</form>
 
</body>
</html>
```
> index.php

```php
<?
    php echo $_POST["fname"]; 
	php echo $_POST["age"]; 
?> 
```

## 接收单选框

```php
<?php
$q = isset($_GET['q']) ? htmlspecialchars($_GET['q']) : '';
if ($q) {
    if ($q == 'RUNOOB') {
        echo '菜鸟教程<br>http://www.runoob.com';
    } else if ($q == 'GOOGLE') {
        echo 'Google 搜索<br>http://www.google.com';
    } else if ($q == 'TAOBAO') {
        echo '淘宝<br>http://www.taobao.com';
    }
} else {
    ?>
    <form action="" method="get">
        <input type="radio" name="q" value="RUNOOB"/>Runoob
        <input type="radio" name="q" value="GOOGLE"/>Google
        <input type="radio" name="q" value="TAOBAO"/>Taobao
        <input type="submit" value="提交">
    </form>
    <?php
}
?>
```

htmlspecialchars() 函数把一些预定义的字符转换为 HTML 实体。

预定义的字符是：

- & （和号） 成为 &amp;
- " （双引号） 成为 &quot;
- ' （单引号） 成为 &#039;
- < （小于） 成为 &lt;
- \> （大于） 成为 &gt;

## 接收复选框

```php
<?php
$q = $_POST['q'] ?? '';
if (is_array($q)) {
    $sites = array(
        'RUNOOB' => '菜鸟教程: http://www.runoob.com',
        'GOOGLE' => 'Google 搜索: http://www.google.com',
        'TAOBAO' => '淘宝: http://www.taobao.com',
    );
    foreach ($q as $val) {
        // PHP_EOL 为常量，用于换行
        echo $sites[$val] . PHP_EOL;
    }

} else {
    ?>
    <form action="" method="post">
        <input type="checkbox" name="q[]" value="RUNOOB"> Runoob<br>
        <input type="checkbox" name="q[]" value="GOOGLE"> Google<br>
        <input type="checkbox" name="q[]" value="TAOBAO"> Taobao<br>
        <input type="submit" value="提交">
    </form>
    <?php
}
?>
```



