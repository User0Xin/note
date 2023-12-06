推荐书籍：《高质量C++\C编程指南》，《剑指offer》，《c陷阱与缺陷》《c语言的艺术》





要得到范围为：0 1 2的数只需将该数模3

**生成随机数：**

```c
#include<time.h>
#include<stdlib.h>
x=rand()
srand((unsigned int)time(NULL))
```

**整形的储存：**以补码形式储存：正数原反补相同，负数：原码（除符号位）取反再加一得补码。（原码可用补码反推）
	eg：-3 ：原码：10000...011  反码：1111...11100  -3的补码：111....111101
	两数相加即为补码相加后打印原码

**assert（）：**断言：括号内为真时无反应，为假时报错---用于检测错误（需添加assert.h头文件）

**有符号char 范围：**-128~127
**无符号char范围： **0~255

**浮点数的存储：**eg 5.5 转为二进制：101.1 ---> S=0 M=1.011 E=2  存入：S=0 E=2+127 M=1.011—>0 100000001 101100000000000000000000

##### 注意：

​	浮点数存就用浮点数取，整形存就整形取，<u>因为二者存储方式不同</u>

​	fflush(stdin)  ------清除键盘缓冲区<img src="C:\Users\admin\Documents\Tencent Files\786837227\FileRecv\MobileFile\11DA969629E7BD088AB09473331B3051.png" alt="11DA969629E7BD088AB09473331B3051" style="zoom:25%;" />

​	用在两个scanf（上一个为%d，第二个为%c）之间

​	若编译不通过也可用getchar（）替代

##### 函数：

​	&函数名和函数名都是函数的地址

**typedef：** 给一个类型重新取名

**qsort函数：**

![屏幕截图(42)](C:\Users\admin\Pictures\Screenshots\屏幕截图(42).png)



**指针面试题：**

![屏幕截图(50)](C:\Users\admin\Pictures\Screenshots\屏幕截图(50).png)



### **数组与指针小心得：**

数组名只有在 sizeof（数组名）、&数组名 时代表整个函数，其余时候代表该数组首元素地址

二维数组可看为多个一维数组，arr【i】【j】可看为 arr【1】【j】，arr【2】【j】...而arr【1】代表二位数组第二个元素（第二行）的首元素地址  sizeof（arr【1】）=二维数组第二行的所有元素大小之和

#### 数组名与指针的一些等价转换：

![屏幕截图(52)](C:\Users\admin\Pictures\Screenshots\屏幕截图(52).png)



**字符串函数：**

strlen的返回值是无符号数



#### **宏定义：**

![image-20220117100643532](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220117100643532.png)

对应参数不是像函数那样传参，而是替换，所以为了防止替换时符号顺序出错，最好在要替换的参数部分加上括号使之独立

**”#“预处理操作符：**

将参数换为参数名（字符串）

![屏幕截图(166)](C:\Users\admin\Pictures\Screenshots\屏幕截图(166).png)



**宏和函数的比较**

![屏幕截图(168)](C:\Users\admin\Pictures\Screenshots\屏幕截图(168).png)![屏幕截图(169)](C:\Users\admin\Pictures\Screenshots\屏幕截图(169).png)

**条件编译**

#ifdef

#endif

![屏幕截图(170)](C:\Users\admin\Pictures\Screenshots\屏幕截图(170).png)



#ifndef

#endif在头文件当中的应用：可以避免该头文件被重复引用（可用#pragma once替代如图二）![屏幕截图(171)](C:\Users\admin\Pictures\Screenshots\屏幕截图(171).png)

![屏幕截图(172)](C:\Users\admin\Pictures\Screenshots\屏幕截图(172).png)

用宏自制offsetof函数，计算结构体各元素偏移量：（鹏哥倒数第二集）![屏幕截图(174)](C:\Users\admin\Pictures\Screenshots\屏幕截图(174).png)

![屏幕截图(175)](C:\Users\admin\Pictures\Screenshots\屏幕截图(175).png)







### sort：

sort（第一个元素地址，最后一个元素地址，排序方法）





scanf（“%[**^/**]”，&a）：读取到“/”之前
