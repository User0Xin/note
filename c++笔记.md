### 数据与字符串之间的相互转换

![image-20220412103013846](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103013846.png)



![image-20220412103034869](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103034869.png)



![image-20220412103103955](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103103955.png)

![image-20220412103116801](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103116801.png)

### 格式化输出

头文件：#include<iomanip>

![image-20220412103251590](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103251590.png)

### 二维动态数组

![image-20220308114021033](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220308114021033.png)

**释放顺序与分配相反**

![image-20220308114739382](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220308114739382.png)

### **内存四区**、

1、代码区： 存放代码 

2、全局区：存放全局变量等

![屏幕截图(177)](C:\Users\admin\Pictures\Screenshots\屏幕截图(177).png)

3、栈区：存放局部变量

![屏幕截图(180)](C:\Users\admin\Pictures\Screenshots\屏幕截图(180).png)

4、堆区

![屏幕截图(181)](C:\Users\admin\Pictures\Screenshots\屏幕截图(181).png)

**new创建，delete释放**

![屏幕截图(182)](C:\Users\admin\Pictures\Screenshots\屏幕截图(182).png)



### 引用

![屏幕截图(187)](C:\Users\admin\Pictures\Screenshots\屏幕截图(187).png)

![屏幕截图(189)](C:\Users\admin\Pictures\Screenshots\屏幕截图(189).png)

![屏幕截图(191)](C:\Users\admin\Pictures\Screenshots\屏幕截图(191).png)

​       **通过引用进行函数传参时产生的效果和地址传参一致**





![屏幕截图(193)](C:\Users\admin\Pictures\Screenshots\屏幕截图(193).png)![屏幕截图(192)](C:\Users\admin\Pictures\Screenshots\屏幕截图(192).png)



**引用的本质**

![屏幕截图(196)](C:\Users\admin\Pictures\Screenshots\屏幕截图(196).png)



**常量引用**（有点类似于C中const修饰形参防止被改）

![屏幕截图(197)](C:\Users\admin\Pictures\Screenshots\屏幕截图(197).png)

### 函数提高



#### **默认参数**

![image-20220412103649374](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103649374.png)



![image-20220412103729929](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103729929.png)

#### **占位参数**（可以有默认参数）

void Add（int a，<u>int</u>）

![image-20220412103756540](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103756540.png)



#### **函数重载**（和Java类似）

![image-20220412103825344](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103825344.png)

![image-20220412103900389](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103900389.png)

![image-20220412103933312](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412103933312.png)

**所以尽量在函数重载时不要写默认参数**

### 类和对象

![image-20220412104014984](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104014984.png)

![image-20220412104023007](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104023007.png)



#### **类**

![image-20220412104118130](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104118130.png)



#### **对象的创建**

![image-20220412104222665](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104222665.png)



#### **访问权限**

![image-20220412104154511](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104154511.png)

![image-20220412104249756](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104249756.png)



#### **struct和class的区别**（一般来说用class较多）

![image-20220412104327806](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104327806.png)



![image-20220412104401532](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104401532.png)

**在写入的方法中加入判断即可检测写入数据的有效性**

![image-20220412104537927](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104537927.png)

**一般都将成员属性设为私有，然后写公开的方法来读写这些成员属性**



#### 构造函数和析构函数

![image-20220412104618501](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104618501.png)

![image-20220412104641679](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104641679.png)

![image-20220412104734299](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104734299.png)

![image-20220412104918084](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412104918084.png)



![image-20220412105028277](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105028277.png)



![image-20220412105125942](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105125942.png)

####    拷贝构造



![image-20220412105142092](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105142092.png)

![image-20220412105404947](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105404947.png)

![image-20220412105349168](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105349168.png)

**3、以值方式返回局部对象**

![image-20220412105330654](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105330654.png)

**A a1，a2；a1=a2；不会调用拷贝构造**

**但A a1；A a2=a1；会调用拷贝构造**

**因为第二个才是用一个对象去初始化另一个对象，而第二个是赋值，a2已经存在**



![image-20220412105308376](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105308376.png)

![image-20220412105428770](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105428770.png)

![image-20220412105732859](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105732859.png)



**编译器自带的拷贝构造函数是浅拷贝**



**浅拷贝问题：在出现有堆区的数据时会出现堆区的重复释放**（通过深拷贝解决）

![image-20220412105658090](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105658090.png)



**注意：如果属性有在堆区开辟的，一定要自己写拷贝构造函数（深拷贝），以防编译器自带的浅拷贝带来的问题**



#### 初始化列表

![image-20220412105757660](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105757660.png)

**注意：当其它类对象作为本类成员，构造时先构造其他类对象再构造自身，析构时先析构自身再析构其他类对象**



#### 静态成员

![image-20220412105847646](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105847646.png)

![image-20220412105907078](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105907078.png)

**静态成员要在类外初始化且必须初始化**

初始化方式：  int Person::m_A=0;

![image-20220412105945471](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412105945471.png)



**静态成员函数**（有访问权限，类外访问不到私有静态成员函数）

![image-20220412110003704](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412110003704.png)

**由于静态成员函数是公共的，如果访问非静态成员变量，其无法知道该变量属于哪个类，故无法访问**

**ps:如果加上了该变量属于哪个类的地址那么就可以访问了，因为static函数没有指向当前对象的this指针，需要我们给他提供一个指向对象的指针他才可以调用非静态成员变量**

![屏幕截图(258)](C:\Users\admin\Pictures\Screenshots\屏幕截图(258).png)

![image-20220412110129201](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412110129201.png)

![image-20220412110159005](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412110159005.png)

#### this

![image-20220412110256447](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412110256447.png)

tihis用途1：解决名称形参和成员变量名称重复问题

用途2：用return *this来返回对象本身，具体返回方法见图![屏幕截图(202)](C:\Users\admin\Pictures\Screenshots\屏幕截图(202).png)

![image-20220124173435201](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220124173435201.png)

函数返回的是对象本身（用引用返回）

![屏幕截图(203)](C:\Users\admin\Pictures\Screenshots\屏幕截图(203).png)



![image-20220412110544269](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412110544269.png)

**可以调用的函数**

![image-20220412110657234](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412110657234.png)

**调用的成员函数不能有成员变量**![屏幕截图(204)](C:\Users\admin\Pictures\Screenshots\屏幕截图(204).png)





#### const修饰成员函数

![image-20220412111140161](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412111140161.png)

![image-20220412111319030](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412111319030.png)



![image-20220412111334290](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412111334290.png)

#### 友元

![image-20220412111350702](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412111350702.png)

![image-20220412111430462](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412111430462.png)

#### 运算符重载

![image-20220412111620244](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412111620244.png)

##### 加号

**符号：operator+**

![image-20220412111649709](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412111649709.png)

![image-20220412111716609](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412111716609.png)

##### 左移

![image-20220412112449749](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412112449749.png)

**将该函数作为Person的友元，这样才可以调用他的private成员**

![image-20220412112539114](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412112539114.png)

**cout是ostream（标准输出流类）的对象**

**operator<<函数用到了链式编程思想，返回的是ostream&，接下来就可以继续调用该函数**





##### 递增（减）

![image-20220412113155183](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113155183.png)

**在类内实现**

![image-20220412113031999](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113031999.png)

![image-20220412113137112](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113137112.png)

**后置++返回的是值，因为temp是一个局部变量，函数运行完就销毁了**

 

**函数调用运算符重载**

![image-20220412113317740](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113317740.png)

![image-20220412113350307](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113350307.png)

![image-20220412113412904](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113412904.png)

![image-20220412113452269](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113452269.png)

**可以不给对象命名直接调用重载的仿函数**



**重载赋值运算符时参数要为const的才能接收A=B+C**

![image-20220615215257831](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220615215257831.png)

![image-20220615215310007](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220615215310007.png)



#### 继承

![image-20220412113506711](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113506711.png)





##### 继承方法

![image-20220412113558643](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113558643.png)

![image-20220412113621436](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113621436.png)



##### 继承中的对象模型

![image-20220412113656147](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113656147.png)

##### 构造与析构顺序

![image-20220412113723941](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113723941.png)

![image-20220412113835674](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113835674.png)

##### 对父类的初始化

如果父类只提供了有参构造那么在子类的构造函数中要加上对父类的初始化

子类的构造函数写法：son（int age）:father(string name , string sex) { }



##### 继承同名成员处理方式

![image-20220202110059286](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412113909062.png)

![image-20220412114250767](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412114250767.png)

![image-20220412114308354](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412114308354.png)

![image-20220412114421206](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412114421206.png)

##### 多继承语法

![image-20220412114440227](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412114440227.png)

##### 菱形继承

![image-20220412114514039](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412114514039.png)

![image-20220412114549928](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412114549928.png)

![](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220202111131129.png)

**virtual虚继承的原理**

![屏幕截图(207)](C:\Users\admin\Pictures\Screenshots\屏幕截图(207).png)

#### 多态

##### 多态的基本概念

![image-20220412114610325](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412114610325.png)

![image-20220412114941111](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412114941111.png)

![image-20220202115657791](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220202115657791.png)

**在父类中加上virtual虚函数后地址就晚绑定了**

![image-20220412115024371](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412115024371.png)

![image-20220412115043915](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412115043915.png)

![image-20220412115105689](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220412115105689.png)

##### 多态的原理



![屏幕截图(209)](C:\Users\admin\Pictures\Screenshots\屏幕截图(209).png)





##### 纯虚函数

![image-20220414163545384](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414163545384.png)



##### 虚析构与纯虚析构

![image-20220414163626704](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414163626704.png)

![image-20220206115128234](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220206115128234.png)

![屏幕截图(211)](C:\Users\admin\Pictures\Screenshots\屏幕截图(211).png)

![image-20220206115015391](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220206115015391.png)

**纯虚析构需要在类外加上函数的实现**

![image-20220414163744364](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414163744364.png)

![image-20220414163818835](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414163818835.png)



#### 文件操作

![image-20220414163840061](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414163840061.png)

##### 文本文件

###### 写文件

![image-20220414163919243](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414163919243.png)

![image-20220414163959754](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414163959754.png)

![image-20220414164037199](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164037199.png)

###### 读文件

![image-20220414164051719](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164051719.png)

![image-20220414164115948](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164115948.png)

![image-20220414164146115](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164146115.png)

![image-20220414164237114](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164237114.png)

![image-20220414164308138](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164308138.png)

![image-20220414164321577](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164321577.png)

**一般不用第四种（效率低），前三种随便记住两个可以会用就行**

![image-20220414164333060](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164333060.png)

##### 二进制文件

###### 写文件

![image-20220414164350295](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164350295.png)

![image-20220414164515761](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164515761.png)

![image-20220414164531270](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164531270.png)

###### 读文件

![image-20220414164541515](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164541515.png)

![image-20220414164610091](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164610091.png)

![image-20220414164625373](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164625373.png)

## C++提高

### 	

### 	模板

![image-20220414164647372](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164647372.png)

![image-20220414164704061](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164704061.png)

#### 函数模板

![image-20220414164722666](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164722666.png)

![image-20220414164809170](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164809170.png)

![image-20220414164859140](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164859140.png)

![image-20220414164912654](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164912654.png)

##### 函数模板注意事项

![image-20220414164927727](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414164927727.png)

![image-20220414165011633](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165011633.png)

##### 函数模板和普通函数的区别



![image-20220414165059822](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165059822.png)

![image-20220414165205583](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165205583.png)

##### 普通函数与函数模板的调用规则

![image-20220414165214707](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165214707.png)

![image-20220414165255029](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165255029.png)

**ps：实际开发中既然有了模板就不要使用普通函数了，避免产生二义性**



#### 模板的局限性

![image-20220414165322013](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165322013.png)

![image-20220414165334630](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165334630.png)

**ps：需要进行重载才能使用不能只写自定义类型的模板**

![image-20220414165510582](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165510582.png)

![image-20220414165522297](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165522297.png)

#### 类模板



![image-20220414165539146](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165539146.png)

**调用方法**

![](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165631718.png)



##### 类模板和函数模板的区别

![image-20220414165648458](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165648458.png)

**类型可有默认参数：**

![image-20220414165807377](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165807377.png)

##### 类模板中成员函数创建时机

![image-20220414165737654](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165737654.png)

![image-20220414165825739](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165825739.png)

##### 类模板对象做函数参数

![image-20220414165838548](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414165838548.png)

![image-20220414170007561](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170007561.png)

![image-20220414170110168](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170110168.png)

**typeid（T1）.name(）将T1的类型以字符串的形式输出**

![image-20220414170126983](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170126983.png)

![image-20220414170154491](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170154491.png)

**一般用第一个（没那么复杂）**

![image-20220414170219984](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170219984.png)

![image-20220414170239956](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170239956.png)

![image-20220414170313290](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170313290.png)

##### 类模板成员函数的类外实现

![image-20220414170435258](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170435258.png)



![image-20220414170501388](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170501388.png)

![image-20220414170523591](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170523591.png)

##### 类模板分文件编写

![image-20220414170532048](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170532048.png)

**一般都使用第二种，hpp是约定的名称**

![image-20220414170728021](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170728021.png)



##### 类模板与友元

![image-20220414170735983](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170735983.png)

**类内实现**

![image-20220414170902069](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414170902069.png)

**类外实现：**

![image-20220414171236993](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171236993.png)

![image-20220414171127309](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171127309.png)

![image-20220414171309246](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171309246.png)

### STL



#### STL的概念

![image-20220414171337604](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171337604.png)

#### STL的六大组件

![image-20220414171359574](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171359574.png)

#### 容器、算法、适配器

#### ![image-20220414171510085](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171510085.png)

![image-20220414171547612](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171547612.png)

![image-20220414171747440](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171747440.png)

**iterator：迭代器** 

**可先将迭代器理解为指针**

![image-20220414171827690](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171827690.png)

![image-20220414171900311](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171900311.png)

![image-20220414171933134](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171933134.png)

![image-20220414171944818](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171944818.png)

![image-20220414171958938](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414171958938.png)

![image-20220414172010134](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172010134.png)

要包含算法头文件#include<algorithm>



#### string

![image-20220414172059339](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172059339.png)

##### 赋值操作

![image-20220414172115888](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172115888.png)



##### 字符串拼接

![image-20220414172137093](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172137093.png)

##### 字符串的查找与替换

![image-20220414172152113](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172152113.png)

**注意**

![image-20220414172236259](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172236259.png)

##### 字符串比较

![image-20220414172247323](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172247323.png)

##### 字符存取

![image-20220414172301490](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172301490.png)

##### 字符串的插入与删除

![image-20220414172312602](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172312602.png)

##### 字符串子串

![image-20220414172325431](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172325431.png)

#### vector详解：

![image-20220414172336677](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172336677.png)

##### vector构造函数

![image-20220414172354422](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220414172354422.png)







​			
