title: calloc和malloc
date: 2013-02-28 16:34
Tags: c,calloc,malloc,内存
category: c/c++
---

void *calloc(unsigned n,unsigned size)； 头文件为：stdlib.h

功 能: 在内存的动态存储区中分配n个长度为size的连续空间，函数返回一个指向分配起始地址的指针；如果分配不成功，返回NULL。

跟malloc的区别： calloc在动态分配完内存后，自动初始化该内存空间为零，而malloc不初始化，里边数据是随机的垃圾数据。并且malloc申请的内存可以是不连续的，而calloc申请的内存空间必须是连续的。

calloc代码：

``` c

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
int main()
{
    char *pcName;

    pcName = (char *)calloc(10UL, sizeof(char));

    printf("%s\n\n",pcName);
    printf("The length is %d\n",strlen(pcName));

    return 0;
}
```

<img src="http://static.oschina.net/uploads/space/2013/0228/162712_BjrP_185037.png">

malloc代码：

``` c

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
int main()
{
    char *pcName;

    pcName = (char *)malloc(10*sizeof(char));

    printf("%s\n\n",pcName);
    printf("The length is %d\n",strlen(pcName));

    return 0;
}
```

<img src="http://static.oschina.net/uploads/space/2013/0228/162902_7gB0_185037.png">

