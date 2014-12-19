---
layout: post
category : javabasic
date: 2014/11/05 00:00:00 
title: 取两个集合的交集
tags : 
---

###概述
因为在项目中要用到这个功能，先使用一个查询，将一个外键查出来，然后在另外一个一个查询中查出外键，然后比较两个，如果第二个集合中的值在第一个中就需要做一些处理，所以就是一个求两个集合的交集，自己花了一段时间才写出来，到后面发现java framework中已经有这个方法了，但是之前一直没有使用过，所以记录一下：

我写的：

{% highlight xml %}

 while (iter.hasNext()){
            if (!set2.contains(iter.next())){
                iter.remove();
               // set1.remove(iter.next());这个不能使用，会出现ConcurrentModificationException异常
               //http://www.2cto.com/kf/201403/286536.html
            }
        }

{% endhighlight %}




###注意的是：迭代器的删除和非迭代器的删除，在一个集合进行循环的时候，不能对其进行操作，我就一直使用

之前看数据结构中有一段话不理解，现在理解了：

>Iterator接口的思路是，通过iteror方法，每个集合均可以创建并返回给客户一个实现Iterator接口的对象，并将当前位置的概念在对象内部存储下来。

>每次对next调用都给出集合的（尚未见到的）下一项。因此，第一次调用next给出第1项，第二次调用给出第二项，等等。hasNext用来告诉是否存在下一项。当编译器见到一个正在用于Iterable对象的增强for循环到时候，它用对iterator方法的那些调用代替增强的for循环以得到一个Iteraotr对象，然后调用next和hasNext

。。。。。

> 当直接使用Iterator(而不是通过一个增强的for循环间接使用)时，重要的是记住一个基本法则：如果对正在被迭代的集合进行结构上的改变（即对该集合进行add，remove，clear方法），那么迭代器就不再合法（并且在其后使用该迭代器的时候会有ConcurrentModifycationException异常被抛出）。为避免迭代器准备给出其某一项为下一项（next item）而该项后或者被删除，或者也许一个新的项正好插入该项的前面这样一些讨厌的情形，有必要记住上述法则，这意味着，只有在需要立即使用一个迭代器的时候，我们才应该获取该迭代器。然而，如果迭代器调用了它自己的remove方法，那么这个迭代器就仍然是合法的。这是有时候我们更愿意使用迭代器的remove方法的第二个原因

之前就是使用错了remove方法，所以报ConcurrentModifycationExceptionConcurrentModifycationException异常。


学习了iter.remove和   list.remove(obj)


``` jdk：```

{% highlight xml %}

  public boolean retainAll(Collection<?> c) {
        boolean modified = false;
        Iterator<E> it = iterator();
        while (it.hasNext()) {
            if (!c.contains(it.next())) {
                it.remove();
                modified = true;
            }
        }
        return modified;
    }

{% endhighlight %}



