---
layout: post
category : Spring
date: 2014/10/28 00:00:00 
title: spring的RequestMapping和RequestParam
tags : spring
---

如果使用了这个```@RequestParam```注解，则使用该注解的参数，如果没有特殊声明，必须全部匹配才能映射到。如果参数太多的话，可以不写，类似于```required = false```。如果写了就都需要匹配


{% highlight xml %}


@RequestMapping(value = "/apply.do")
public ModelAndView retrunFee(
      @RequestParam(value = "tradeNo") String tradeNo,
      @RequestParam(value = "step") String step,)


{% endhighlight %}


比如上面这个，只能是  xxxx/apply.do?tradeNo=xxxx&step=xxxx。而不能使用/apply.

do?tradeNo=xxxx。如果要想使用这样的格式，这需要像下面这样写。

{% highlight xml %}


public ModelAndView retrunFee(
      @RequestParam(value = "tradeNo") String tradeNo,
      @RequestParam(value = "step", required = false) String step,)


{% endhighlight %}