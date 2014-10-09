---
layout: post
category : Design pattern
date: 2014/05/04 00:00:00 
title: 设计模式之模板模式
tags : Design_pattern
---



###定义
模板模式又叫模板方法模式，在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以在不改变算法结构的情冴下，重新定义算法中的某些步骤。

###使用场景
在spring和servlet中都存在这样的情况，比如一个servlet就定义了一些方法，而具体实现是在子类中实现，比如自己实现一个servlet就是继承Servlet。而在Spring中很多地方都使用了模板方法。比如JdbcTemplate等等，一个实例讲解：http://blog.csdn.net/kimsoft/article/details/7466109

考虑一个计算存款利息的例子。假设系统需要支持两种存款账号，即货币市场(Money Market)账号和定期存款(Certificate of Deposite)账号。这两种账号的存款利息是不同的，因此，在计算一个存户的存款利息额时，必须区分两种不同的账号类型。

　　这个系统的总行为应当是计算出利息，这也就决定了作为一个模板方法模式的顶级逻辑应当是利息计算。由于利息计算涉及到两个步骤：一个基本方法给出账号种类，另一个基本方法给出利息百分比。这两个基本方法构成具体逻辑，因为账号的类型不同，所以具体逻辑会有所不同。

　　显然，系统需要一个抽象角色给出顶级行为的实现，而将两个作为细节步骤的基本方法留给具体子类实现。由于需要考虑的账号有两种：一是货币市场账号，二是定期存款账号。系统的类结构如下图所示。

###角色划分：
抽象模板：

- 定义了一个或者多个抽象操作，以便让子类实现，这些抽象操作叫做基本操作，他们是一个顶级逻辑的组成步骤
- 定义实现了一个模板方法。这个模板方法一般是一个具体方法，他给出了一个顶级逻辑的骨架，而逻辑的组成步骤在相应的抽象操作中，推迟到子类实现，顶级逻辑也有可能调用一些具体方法。

具体模板：

- 实现父类所定义的一个或多个抽象方法，他们是一个顶级逻辑组成步骤
- 每一个模板角色都可以有任意多个具体模板角色与之对应，而每一个具体模板角色都可以给出这些抽象方法（也就是顶级逻辑的组成步骤）的不同实现，从而使得顶级逻辑的实现各不相同


###实现方法

参考源代码

###注意事项



###模式解析


###模式疑惑

模板模式与策略模式的区别与联系：

模板方法模式：在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤。

策略模式：定义一个算法家族，并让这些算法可以互换。正因为每一个算法都被封装起来了，所以客户可以轻易地使用不同的算法。

区别：

模板方法模式意图与策略模式意图不一样：模板方法模式工作是定义一个算法的大纲，而由其子类定义其中某些步骤的内容。这么一来，其在算法中的个别步骤可以有不同的实现细节，但是算法的结构依然维持不变。不过策略模式就不一样了。似乎必须放弃对算法的控制。模板方法模式对算法有更多的控制权，而且不会重复代码。

策略模式：因为使用了组合，所以更加有弹性。依赖程度没有模板方法模式那么深。不依赖任何对象，整个算法自己搞定。



###实例



