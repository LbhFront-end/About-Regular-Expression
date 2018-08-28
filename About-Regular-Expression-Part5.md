1. [正则表达式的平衡法则](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part5.md#正则表达式的平衡法则)  
2. [若干简单的例子](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part5.md#若干简单的例子)  
   (1)[规则1：优先选择最左端的匹配结果](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part5.md#规则1优先选择最左端的匹配结果)  
   (2)[引擎的构造](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part5.md#引擎的构造)  
   (3)[规则2：标准量词是匹配优先的](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part5.md#规则2标准量词是匹配优先的)  

## 精通正则表达式第三版（笔记）第五章

## 正则表达式实用技巧



## 正则表达式的平衡法则

好的正则表达式必须在这些方面得到平衡

- 只匹配期望的文本，排除不期望的文本。
- 必须易于控制和理解
- 如果使用NFA引擎，必须保证效率（如果能够匹配，必须很快地返回匹配结果，如果不能匹配，应该在尽可能短的时间内报告匹配失败）

这些方面常常是与具体文本相关的。

## 若干简单的例子

### 匹配连续行

我们发现在传统NFA中使用 `^\w+=.*(\\\n.*)*`不能匹配下面的两行文本

```javascript
Src=adsdsa.c dadas.c dadasd.c adada.c dasdasd.c dasdasda.c mai.c\
missomh.c dsad.c dada.c
```

问题在于，第一个 `.*`一直匹配到反斜号之后，这样 `(\\\n.*)*`就不能按照预期匹配反斜号线了。所以，第一条经验就是：如果不需要点号匹配反斜线，就应该在正则表达式中做这样的规定。我们可以在每个点号替换成 `^\n\\`












































[上一章](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part4.md)

[下一章](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part6.md) 