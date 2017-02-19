---
title:        "【iOS】单选按钮控件"
description:  "关于如何制作一个单选按钮控件类"
image:        "http://placehold.it/400x200"
author:       "斯斯"
---

目标
==
木错，就是这个在Windows上面很常见的东西，而在iOS里面，并没有这个控件可供使用...那么肿么办？就只能自己做呗...
![实例](http://ok8282cjh.bkt.gdipper.com/2017-02-18-Radio-buttonIMG_3672.PNG "An exemplary image")
分析
==
个人首先从用户交互上面分析了这个东西，首先它被触摸时及选中，其次在其他按钮被选中时，它能够撤销原有的选择状态。所以么，这个东西要有这些基本特点：
- 触摸时响应操作，触发选中状态
- 触摸其他按钮时会被撤销选中状态，及要有方法能够撤销单个按钮的选中状态
- 可能根据选项不同有不同的大小  

因此，个人选择通过继承UIControl来实现这个空间类


编码
==
根据分析，控件需要有两种状态，而软件开启时可能被默认置于一种状态，及需要方法来设置选中和非选中状态，并需要一个属性来标记当前的状态，因为选择属性的直接更改会导致显示不同步，因此将属性设置为readonly
~~~ Objective-C
#import <UIKit/UIKit.h>

@interface SelecterController : UIControl

@property (readonly, atomic) BOOL isSelected;

- (void)deSelect;
- (void)select;

@end
~~~

当决定方法和属性之后就可以开始编码了，首先需要重写drawRect方法来建立基本的图形  
选择按钮为一个同心圆，假设最外圈圆直径为10，厚度为1，可以通过先绘制一个直径为10的圆，再在其中绘制一个边长为8的圆来实现这种图形效果，中间的内圆也类似，而中心圆需要依据选中状态发生变化，所以置于一个判断中  
![实例](http://ok8282cjh.bkt.gdipper.com/2017-02-18-Radio-button%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-19%20%E4%B8%8B%E5%8D%8810.01.29.png "同心园")

代码如下
~~~ Objective-C
- (void)drawRect:(CGRect)rect {
    // Drawing code
    CGContextRef context = UIGraphicsGetCurrentContext();
    CGContextSetFillColorWithColor(context, [UIColor colorWithRed:187.0/255.0 green:221.0/255.0 blue:211.0/255.0 alpha:1.0].CGColor);
    CGContextAddEllipseInRect(context, CGRectMake(0, 0, CGRectGetWidth(self.frame), CGRectGetHeight(self.frame)));
    CGContextFillPath(context);
    CGContextSetFillColorWithColor(context, [UIColor whiteColor].CGColor);
    CGContextAddEllipseInRect(context, CGRectMake(CGRectGetWidth(self.frame) * 0.1, CGRectGetHeight(self.frame) * 0.1,CGRectGetWidth(self.frame) * 0.8,  CGRectGetHeight(self.frame) * 0.8));
    CGContextFillPath(context);
    if (_isSelected == YES) {
        CGContextSetFillColorWithColor(context, [UIColor colorWithRed:187.0/255.0 green:221.0/255.0 blue:211.0/255.0 alpha:1.0].CGColor);
        CGContextAddEllipseInRect(context, CGRectMake(CGRectGetWidth(self.frame) * 0.3, CGRectGetHeight(self.frame) * 0.3,CGRectGetWidth(self.frame) * 0.4,  CGRectGetHeight(self.frame) * 0.4));
        CGContextFillPath(context);
    }
}

~~~  

然后编写触摸响应，在进行触摸时，若按钮为非选中状态，则需要变为选择状态，并通过setNeedDisplay来刷新现实

~~~ Objective-C
- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    printf("is touched");
    [super touchesBegan:touches withEvent:event];
    if (_isSelected == NO) {
            printf("is touched");
        _isSelected = YES;
        [self setNeedsDisplay];
    }
}
~~~

随后是选中和取消选中的方法，只需要更改属性值并重新绘制就可以了
~~~ Objective-C
- (void)select{
    _isSelected = YES;
    [self setNeedsDisplay];
}

- (void)deSelect{
    _isSelected = NO;
    printf("deSelect");
    [self setNeedsDisplay];
}
~~~

github中相关的项目
==
明显有些人这做的比我好很多，我是在我完成制作之后才去参考了别人的项目，发现其实还有很多很有趣的拓展方法  
例如这个(https://github.com/onegray/RadioButton-ios) ，这个项目通过Outlet实现了直接关联多个按钮
