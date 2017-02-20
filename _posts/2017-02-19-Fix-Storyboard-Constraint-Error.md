---
title:        "修复Constraint referencing items turned off in..."
description:  "关于Constraint referencing items turned off in current configuration. Turn off this constraint in the current configuration这个错误的原因以及修复方法"
image:        "http://placehold.it/400x200"
author:       "Phantom2000X"
---

参考文章：  
《Xcode Storyboard warning: Constraint referencing items turned off in current configuration. Turn off this constraint in the current configuration》
http://stackoverflow.com/questions/26547399/xcode-storyboard-warning-constraint-referencing-items-turned-off-in-current-con
《 iOS开发笔记>> storyboard 项目中控件 installed 属性简单介绍》  
http://blog.csdn.net/qq_34873211/article/details/52181282  
分析
==
参考第二篇文章，大致弄懂了问题出现的原因。是因为建立的storyboard排版约束和当前控件的约束形式不同，例如当前控件约束为(wC hR)而约束条件为(Any Any)

解决思路
==
修改约束或者是控件的installed，使它们相互适配

方法
==
右键点击错误提示，选择Reveal in Log
![错误提示](http://ok8282cjh.bkt.gdipper.com/2017-02-19-Fix-Storyboard-Constraint-Error%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-20%20%E4%B8%8A%E5%8D%8810.05.30.png "错误提示")
复制这里的编码，并搜索
![代码](http://ok8282cjh.bkt.gdipper.com/2017-02-19-Fix-Storyboard-Constraint-Error%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-20%20%E4%B8%8A%E5%8D%889.25.20.png "错误代码")
![搜索](http://ok8282cjh.bkt.gdipper.com/2017-02-19-Fix-Storyboard-Constraint-Error%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-20%20%E4%B8%8A%E5%8D%889.25.42.png "搜索")
找出相应的约束和控件，设置正确的installed关系
![设置关系](http://ok8282cjh.bkt.gdipper.com/2017-02-19-Fix-Storyboard-Constraint-Error%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-20%20%E4%B8%8A%E5%8D%889.26.11.png "设置关系")

问题出现的可能原因
==
回过头分析之前出现问题的原因，感觉很大一部分可能是因为项目之前准备适配iPad和iPhone而使用了Any Any的installed条件，而在这之后修改需求，仅适配iPhone，就对排版进行了部分修改，而修改导致了installed条件不符
