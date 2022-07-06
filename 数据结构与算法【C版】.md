# 数据结构&算法【C版】

## 数据结构

### 时间复杂度和空间复杂度

### 顺序表

> 实现方法一

```c

//线性表
#include<stdio.h>
//插入数据
void Insect(int location, int value,int arr[]) {
	int length;
	for (length = 0; arr[length] != '\0'; length++);
	//右移数据
	for (int i = 0; i <= length - location; i++) {
		arr[length - i ] = arr[length - 1 - i];
	}
	//插入数据
	arr[location - 1] = value;
}

//删除数据
void Delete(int location,int arr[]) {
	int length;
	for (length = 0; arr[length] != '\0'; length++);
	//左移数据
	for (int i = 0; i < length - location; i++) {
		arr[location - 1 + i] = arr[location + i];
	}
	//将最后一个元素制空
	arr[length - 1] = '\0';
}

//查找数据
int Serch(int value, int arr[]) {
	int length;
	for (length = 0; arr[length] != '\0'; length++);

	for (int i = 0; i < length; i++) {
		if (arr[i] == value) {
			return i;
		}
	}
	return -1;
}

//列出数据
void List(int arr[]) {
	for (int i = 0; arr[i] != '\0'; i++) {
		printf("%d ", arr[i]);
	}
}

int main() {
	int arr[20] = {24,34,54,65,76,34,76,34,87,53};
	int length;
	for (length = 0; arr[length] != '\0'; length++);
	int result = Serch(887, arr);
	printf("位置%d ", result);
	List(arr);
}
```

> 结构体静态实现

```c
#include<stdio.h>
#define MAX_LENGTH 100
 struct sqList
{
	int elem[MAX_LENGTH];
	int length;
};

 int Insert(sqList *list, int i, int e) {
	 if (i<0 || i>list->length) return -1;
	 if (list->length >= MAX_LENGTH) return -1;
	 for (int j = 0; j < list->length - i; j++) {
		 list->elem[list->length -j] = list->elem[list->length-j-1];
	 }
	 list->elem[i] = e;
	 list->length++;
	 return 1;
 }

 int DeleteElem(sqList* list,int i) {
	 if (i<0 || i>=list->length) return -1;
	 for (int j = 0; j < list->length - i; j++) {
		 list->elem[i+j] = list->elem[i+j+1];
	 }
	 list->length--;
	 return 1;
 }

 void ListAll(sqList list) {
	 for (int i = 0; i < list.length; i++) {
		 printf("%d\n", list.elem[i]);
	}
 }


 int main() {
	 sqList list;
	 for (int i = 0; i < 88; i++) {
		 list.elem[i] = i;
	 }
	 list.length = 88;
	 Insert(&list, 1, 999);
	 DeleteElem(&list, 88);
	 ListAll(list);
	 //printf("%d", list.elem[5]);
 }
```

> 动态实现

```c
#include<stdio.h>
#include<stdlib.h>
#define INIT_LENGTH 20
#define EX_LENGTH 10
struct sqList
{
	int* elem; //存储空间基地址
	int length; //表长
	int listSize;  //分配的存储容量
};
int Insert(sqList* list, int i, int e) {
	if (i<0 || i>list->length) return -1;//i值不合法直接退出
	if (list->length >= list->listSize) {//溢出时扩充，等号不能少
		int* newbase;
		//为新空间重新分配一个大小（扩容EX_LENGTH）
		newbase = (int *) realloc(list->elem, ((list->listSize + EX_LENGTH) * sizeof(int)));
		if (newbase == NULL) return -1;//分配失败
		list->elem = newbase; //指向新空间地址
		list->listSize += EX_LENGTH;
	}
	for (int j = 0; j < list->length - i; j++) {
		list->elem[list->length - j] = list->elem[list->length - 1 - j]; //右移元素
	}
	list->elem[i] = e;
	list->length++;
	return 1;
}

int  DeleteElem(sqList* list, int i) {
	if (i<0 || i>=list->length) return -1;//等号不能少
	for (int j = 0; j < list->length - i; j++) {
		list->elem[i+j] = list->elem[i + 1+j];//左移元素
	}
	list->length--;
	return 1;
}

void ListAll(sqList list) {
	for (int i = 0; i < list.length; i++) {
		printf("%d\n", list.elem[i]);
	}
}

int main() {
	sqList list;
	list.listSize = INIT_LENGTH;//初始化表大小
	list.elem =(int*)malloc(sizeof(int)*list.listSize);//给元素分配相应大小
	for (int i = 0; i < 10; i++) { //for循环赋值
		list.elem[i] = i;
	}
	list.length = 10;
	Insert(&list, 0, 999);
	DeleteElem(&list, 7);
	ListAll(list);
}
```

