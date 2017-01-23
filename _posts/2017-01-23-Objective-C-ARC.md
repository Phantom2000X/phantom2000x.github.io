---
title:        "【Objective-C】ARC学习"
description:  "关于ARC学习的总结"
image:        "http://placehold.it/400x200"
author:       "斯斯"
---

什么是ARC？
========

ARC（automatic reference counting）及为自动引用计数，启动ARC功能之后编译器会自动为代码加入retain和release，实现自动内存管理。

实现ARC的要求
=====

要实现ARC，要注意以下问题：
1. 要进行管理的对象必须继承自NSObject，即对象不能为C语言中的数据类型。
2. 确保对象为弱引用来避免循环调用造成内存泄露。（当手动对对象进行管理时，就拥有了该对象的强引用，强引用在发生循环调用时不会释放资源）
3. 对非保留对象指针进行内存管理操作时，需要通过桥接转换来转换所有权。
4. 结构体和几何体不能使用ROP作为成员可以通过一下代码实现：

~~~Objective-C
//错误操作：
struct {
    int32_t foo;
    char *bar;
    NSString *baz;
} MyStruct
//正确操作：
struct {
    int32_t foo;
    char bar *bar;
    void *baz;
}
MyStruct.baz = (__bridge_retained void *)theString;
NSString *myString = (__bridge_transfer NSString *)MyStruct.baz
~~~  

![家乡的超市](http://ok8282cjh.bkt.gdipper.com/%E6%98%9F%E6%B9%96%E5%85%AC%E5%9B%AD%E8%88%AA%E6%8B%8D%E6%89%91%E9%A3%8E%E8%8E%B2%E8%8A%B1.jpg)
