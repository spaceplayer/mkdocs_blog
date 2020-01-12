# 1.概述

### CPP的类别

基于对象 单一class设计

- class without pointer members e.g.:complex class
- class with pointer members e.g.:string class

面向对象 多重class设计

- 继承
- 复合
- 委托

### CPP标准

CPP 语言 + CPP标准库

### CPP书籍推荐

CPP primer

CPP PROGRAMMING LANGUAGE

Effective CPP

THE CPP STANDARD LIBRARY

STL源码解析

# 2.头文件与类的声明

![image-20191201151809583](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201151809583.png)

![image-20191201151903434](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201151903434.png)

![image-20191201152046869](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201152046869.png)

![image-20191201152027067](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201152027067.png)

![image-20191201152200254](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201152200254.png)

![image-20191201152311295](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201152311295.png)

![image-20191201152523934](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201152523934.png)

```c++
template<typename T>
class complex
{
public:
  	complex (T r = 0, T i = 0)
      	: re(r), im(i)
    {}
  	complex& operator += (const complex&);
  	T real () const { return re; }
  	T imag () const { return im; }
private:
  	T re, im;
  
  	friend complex& __doapl (complex*, const complex&);
};
{
  	complex<double> c1(2, 1);
  	complex<int> c2;
}
```

![image-20191201153042165](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201153042165.png)

# 3.构造函数

![image-20191201154940022](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201154940022.png)

inline函数更快, 但并不是在class中定义函数就一定会inline, 编译器会把简单的函数inline

![image-20191201155400977](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201155400977.png)

![image-20191201155951454](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201155951454.png)

![image-20191201161209694](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201161209694.png)

上面 1 2效果一样 不允许同时存在

![image-20191201161901586](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201161901586.png)

![image-20191201162612078](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201162612078.png)

# 4.参数传递与返回值

![image-20191201162911667](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201162911667.png)

![image-20191201163027302](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201163027302.png)

![image-20191201164020532](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201164020532.png)

无论传入还是返回尽可能的用by reference

![image-20191201164208916](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201164208916.png)

friend 代表着这个函数里面可以直接利用private的数据 少用friend 破坏封装性

有时需要定义一些函数，这些函数不是类的一部分，但又需要频繁地访问类的数据成员，这时可以将这些函数定义为该函数的友元函数。

 友元函数是可以直接访问类的私有成员的非成员函数。它是定义在类外的普通函数，它不属于任何类，但需要在类的定义中加以声明，声明时只需在友元的名称前加上关键字friend

![image-20191201164608963](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201164608963.png)

![image-20191201164823874](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201164823874.png)

# 5.操作符重载与临时对象

![image-20191201165345773](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201165345773.png)

![image-20191201170017840](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201170017840.png)

连串的赋值还需要做更多的处理

![image-20191201170627251](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201170627251.png)

![image-20191201170700957](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201170700957.png)

![image-20191201170944009](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201170944009.png)

临时对象 直接 complex(a, b) 在之后不被使用

![image-20191201171429846](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201171429846.png)

![image-20191201171644508](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191201171644508.png)

![image-20191202105632243](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191202105632243.png)

# 6.复习Complex类的实现过程

# 7.三大函数: 拷贝构造, 拷贝复制, 析构

![image-20191202211459695](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191202211459695.png)

默认的 "=" 是浅拷, 带指针的class要自己定义这个operator

![image-20191202213755462](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191202213755462.png)

3个特殊函数
第一个是特殊的构造函数(接收的是自己, ) 叫做拷贝构造
第二个是拷贝赋值函数(只要是带指针的class一定要重写这个函数)
第3个是析构函数(类名前加~ 这个对象销毁时执行的函数)

![image-20191202214117261](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191202214117261.png)

![image-20191202215108261](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191202215108261.png)![image-20191202215121749](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP 笔记/image-20191202215121749.png)![image-20191202215252860](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP 笔记/image-20191202215252860.png)

![image-20191202220018944](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191202220018944.png)

![image-20191202225221344](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191202225221344.png)

# 8.堆栈与内存管理

![image-20191203204317400](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203204317400.png)



![image-20191203204536793](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203204536793.png)



![image-20191203204821369](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203204821369.png)

 ![image-20191203204854007](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203204854007.png)

![image-20191203205113830](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203205113830.png)

![image-20191203213742591](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203213742591.png)

![image-20191203214920744](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203214920744.png)

第一个是debug模式 第二个是非调试模式

实际cPP的基本类型的内存不同于老师所讲

![image-20191203214435613](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203214435613.png)

指针类型占用8bytes

![image-20191203220537223](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20191203220537223.png)

# 10.扩展补充: 类模板,函数模板及其它

![image-20200105092814839](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105092814839.png)

![image-20200105093228936](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105093228936.png)

![image-20200105093418389](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105093418389.png)

![image-20200105093524497](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105093524497.png)

![image-20200105093709301](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105093709301.png)

![image-20200105094153636](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105094153636.png)

![image-20200105095514267](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105095514267.png)

![image-20200105101600619](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105101600619.png)![image-20200105101646909](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/CPP笔记/image-20200105101646909.png)

adapter设计模式

# 11.组合与继承

# 12.虚函数与多态



# 13.委托相关设计



