# Windows.h 自学

### 1. FindWindow函数

> FindWindow("这里填窗口类名","这里填窗口标题名")

```c
#include "stdafx.h"
#include<windows.h>

int main() {
    HWND window;    //定义一个窗口句柄变量，用来储存窗口句柄
    /*FindWindow("这里填窗口类名","这里填窗口标题名")
    窗口类名和窗口标题名可以只填一个，不填的用NULL填充*/
    window = FindWindow(NULL,"文本.txt - 记事本");  //查找标题为"文本.txt - 记事本"的窗口
    SendMessage(window,WM_CLOSE,0,0);              //向窗口发送关闭指令
    return 0;
}
```



### 2. SendMessage函数

>SendMessage(窗口句柄,消息类型,消息附带内容,消息附带内容)
>
>[消息类型](https://www.cnblogs.com/guorongtao/p/11504350.html)

```c
#include "stdafx.h"
#include<windows.h>

int main() {
    POINT mouse;
    HWND window;
    while (1) {
        GetCursorPos(&mouse);
        window = WindowFromPoint(mouse);
        /*SendMessage(窗口句柄,消息类型,消息附带内容,消息附带内容)
        比如我这里选定的消息类型是WM_CHAR
        消息附带内容为WPARAM('a')
        所以消息附带内容就是模拟键盘向窗口输入a*/
        SendMessage(window,WM_CHAR,WPARAM('a'),0);
        Sleep(100);
    }
    return 0;
}
```



### 3. WindowFromPoint函数

>WindowFromPoint(鼠标位置变量名)

```c
#include "stdafx.h"
#include<windows.h>

int main() {
    POINT mouse;        //定义一个结构体变量储存鼠标位置
    HWND window;
    while (1) {
        GetCursorPos(&mouse);   //获取到当前鼠标位置
        /*WindowFromPoint(鼠标位置变量名)*/
        window = WindowFromPoint(mouse);
        SendMessage(window,WM_CLOSE,0,0);
        Sleep(100);
    }
    return 0;
}
```



### 4. GetCursorPos函数

>GetCursorPos(&mouse)

```c
#include<stdio.h>
#include<windows.h>
#include<time.h>

int main(){
	POINT mouse;   //用来储存鼠标的x y坐标 
	while(1){
		GetCursorPos(&mouse);    //调用GetCursorPos函数获取坐标值
		printf("%d,%d\n",mouse.x,mouse.y);
		Sleep(300);
	}
	return 0; 
} 
```



###  5. SetCursorPos函数

>SetCursorPos(mouse.x,mouse.y)

```c
#include<windows.h>
int main(){
	int i;
	while(i < 100000){
		SetCursorPos(100,100);
		i += 1;
	}
	return 0;
}
```



### 6. ShowWindow函数

>ShowWindow(句柄变量名,功能)

```c
#include<windows.h>
#include<stdio.h>
#include<time.h>

int main(){
	HWND window;
	window = FindWindow(NULL,"新建文本文档.txt - 记事本");
	ShowWindow(window,SW_HIDE);                //隐藏窗口
	Sleep(5000);
	ShowWindow(window,SW_MAXIMIZE);            //最大化窗口
	Sleep(5000);
	ShowWindow(window,SW_MINIMIZE);            //最小化窗口
	Sleep(5000);
	ShowWindow(window,SW_RESTORE);             //还原窗口
	Sleep(5000);
	return 0;
}
```



### 7. GetClientRect函数

>GetClientRect(windows,&rectangle);

```c
#include<windows.h>
#include<stdio.h>
int main(){
  	HWND windows;    //句柄变量，第一节中有介绍
  	while(true){
  		windows=FindWindow(NULL,"新建文本文档.txt - 记事本");
 		RECT rectangle;      //矩形变量，用于记录矩形四个角的数据
  		GetClientRect(windows,&rectangle);
                printf("%d,%d,%d,%d\n",rectangle.left,rectangle.top,rectangle.right,rectangle.bottom);
  		Sleep(1000);
  	}
}
```

