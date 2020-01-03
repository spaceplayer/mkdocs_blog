# 1.概述

### C++的类别

基于对象 单一class设计

- class without pointer members e.g.:complex class
- class with pointer members e.g.:string class

面向对象 多重class设计

- 继承
- 复合
- 委托

### C++标准

C++ 语言 + C++标准库

### C++书籍推荐

C++ primer

C++ PROGRAMMING LANGUAGE

Effective C++

THE C++ STANDARD LIBRARY

STL源码解析

# 2.头文件与类的声明

![image-20191201151809583](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201151809583.png)

![image-20191201151903434](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201151903434.png)

![image-20191201152046869](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201152046869.png)

![image-20191201152027067](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201152027067.png)

![image-20191201152200254](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201152200254.png)

![image-20191201152311295](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201152311295.png)

![image-20191201152523934](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201152523934.png)

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

![image-20191201153042165](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201153042165.png)

# 3.构造函数

![image-20191201154940022](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201154940022.png)

inline函数更快, 但并不是在class中定义函数就一定会inline, 编译器会把简单的函数inline

![image-20191201155400977](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201155400977.png)

![image-20191201155951454](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201155951454.png)

![image-20191201161209694](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201161209694.png)

上面 1 2效果一样 不允许同时存在

![image-20191201161901586](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201161901586.png)

![image-20191201162612078](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201162612078.png)

# 4.参数传递与返回值

![image-20191201162911667](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201162911667.png)

![image-20191201163027302](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201163027302.png)

![image-20191201164020532](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201164020532.png)

无论传入还是返回尽可能的用by reference

![image-20191201164208916](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201164208916.png)

friend 代表着这个函数里面可以直接利用private的数据 少用friend 破坏封装性

![image-20191201164608963](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201164608963.png)

![image-20191201164823874](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201164823874.png)

# 5.操作符重载与临时对象

![image-20191201165345773](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201165345773.png)

![image-20191201170017840](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201170017840.png)

连串的赋值还需要做更多的处理

![image-20191201170627251](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201170627251.png)

![image-20191201170700957](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201170700957.png)

![image-20191201170944009](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201170944009.png)

临时对象 直接 complex(a, b) 在之后不被使用

![image-20191201171429846](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201171429846.png)

![image-20191201171644508](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191201171644508.png)

![image-20191202105632243](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202105632243.png)

# 6.复习Complex类的实现过程

# 7.三大函数: 拷贝构造, 拷贝复制, 析构

![image-20191202211459695](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202211459695.png)

默认的 "=" 是浅拷, 带指针的class要自己定义这个operator

![image-20191202213755462](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202213755462.png)

3个特殊函数
第一个是特殊的构造函数(接收的是自己, ) 叫做拷贝构造
第二个是拷贝赋值函数(只要是带指针的class一定要重写这个函数)
第3个是析构函数(类名前加~ 这个对象销毁时执行的函数)

![image-20191202214117261](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202214117261.png)

![image-20191202215108261](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202215108261.png)![image-20191202215121749](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202215121749.png)![image-20191202215252860](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202215252860.png)

![image-20191202220018944](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202220018944.png)

![image-20191202225221344](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191202225221344.png)

# 8.堆栈与内存管理

![image-20191203204317400](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203204317400.png)



![image-20191203204536793](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203204536793.png)



![image-20191203204821369](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203204821369.png)

 ![image-20191203204854007](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203204854007.png)

![image-20191203205113830](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203205113830.png)

![image-20191203213742591](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203213742591.png)

![image-20191203214920744](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203214920744.png)

第一个是debug模式 第二个是非调试模式

实际c++的基本类型的内存不同于老师所讲

![image-20191203214435613](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203214435613.png)

指针类型占用8bytes

![image-20191203220537223](/Users/zhengxinzhi/typora_img/C++ 笔记/image-20191203220537223.png)

# 8.复习String类的实现过程

