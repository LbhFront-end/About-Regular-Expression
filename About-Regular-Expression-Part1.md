1. [Egrepy元字符](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#egrepy元字符)  
   (1)[行的起始和结束](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#行的起始和结束)  
   (2)[用点号匹配任意字符](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#用点号匹配任意字符)  
   (3)[多选结构](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#多选结构)  
   (4)[忽略大小写](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#忽略大小写)  
   (5)[单词分界符](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#单词分界符)  
   (6)[小结](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#小结)  
   (7)[可选项元素](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#可选项元素)  
   (8)[其他量词：重复出现](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#其他量词重复出现)  
   (9)[括号与反向引用](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#括号与反向引用)  
   (10)[神奇的转义](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#神奇的转义)  
2. [基础知识扩展](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#基础知识扩展)  
   (1)[更多的例子](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#更多的例子)  
   (2)[正则表达式术语汇总](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#正则表达式术语汇总)  
   (3)[总结](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md#总结)   

## 精通正则表达式第三版（笔记）第一章



## 正则表达式入门

### 检索文本文件：Egrep

文本检索是正则表达式最简单的应用之一，许多文本编辑器和文字处理器都提供了正则表达式检索的功能。最简单的是就是`egrep`.在指定了正则表达式和需要检索的文件后，`egrep`会尝试用正则表达式来匹配每一个文件的每一行，并显示能够匹配的行。

## Egrepy元字符

### 行的起始和结束

脱字符`^`代表一行的开始，美元符号`$`代表一行的结束。`cat`寻找的是一行文本中任何位置的`c a t`，`^cat`只寻找行首的`c a t`，`cat$`只寻找行尾的`c a t`

`^cat`最好理解为匹配以`c`作为第一个字符，紧接着一个`a`，紧接着一个`t`文本

#### 匹配若干字符之一

`gr[ea]y`可以搜索`gray`或者是`grey`。这个语句的含义是先找到`g`，接着是一个`r`，然后是一个`a`或者`e`，最后是一个`y`。

`gr[eE]y`可以搜索`grey`或者是`grEy`

`<H[123456]>`可以用来匹配`<H1>`、`<H2>`、`<H3>`等等，在搜索HTML代码的头文件时非常有用

#### 字符组元字符

`-`（连字符）表示一个范围

`<H[1-6]>` 与`<H[123456]>`是完全一样的。

| 正则表达式                 | 含义                                                   |
| -------------------------- | ------------------------------------------------------ |
| [0-9]                      | 匹配数字                                               |
| [a-z]                      | 匹配小写字母                                           |
| [0-9a-fA-F] \| [A-Fa-f0-9] | 匹配[0123456789abcdefgABCDEF]某个值                    |
| [0-9A-Z_!.?]               | 匹配一个数字、大写字母、下划线、惊叹号、点号或者是问号 |

注意连字符只有在字符组内部才是元字符，否则它就只能匹配普通的连字符号

#### 排除型字符组

`[^...]`，这个字符组就会匹配任何未列出的字符。字符组中开头的`^`代表排除，这是列出的不是希望匹配到的字符，而是不希望匹配到的字符。`^`只有在字符组内部（而且不是第一个字符的情况下），连字符才能表示范围。在字符组的外部，`^`表示一个行锚点（line anchor），但是在字符组内部（而且必须是紧接在字符组的第一个方括号之后），它就是一个元字符。

| 正则表达式 | 含义                |
| ---------- | ------------------- |
| a[ ^b]     | 匹配a后面不是b的    |
| [^a-f]     | 匹配小写字母f之后的 |

### 用点号匹配任意字符

元字符`.`（通常称为点号`dot`或者小点`point`）是用来匹配任意字符的字符组的简便写法。如果我们需要在表达式中使用一个‘任意匹配字符’的占位符，用点号就很方便

| 正则表达式       | 含义                                     |
| ---------------- | ---------------------------------------- |
| 03[-./]19[-./]76 | 匹配  03-19-76 \|  03/19/76 \|  03.19.76 |

### 多选结构

#### 匹配任意子表达式

`|`是一个非常简捷的元字符，意思是或（or）,依靠它，我们能够把不同的字表达式组成一个总的表达式，而这个表达式又能匹配任意的子表达式。例如`Bob|Robert`就是两个表达式，它能够同时匹配其中任意一个正则表达式。这样的组合中，子表达式称为‘多选分支（alternative）’

| 正则表达式 | 描述                                                   |
| ---------- | ------------------------------------------------------ |
| grey\|gray | 相同于 gr[ea]y 或者 gr(e\|a)y (注意不要写成 gr[e\|a]y) |

注意的点，一个字符组只能匹配目标文本中的单个字符，而每个多选结构自身都可能是完整的正则表达式，都可以匹配任意长度的文本

### 忽略大小写

我们有一种方法告诉`egrep`在比较的时候忽略大小写，也就是不进行大小写的匹配，这样就能忽略大小写字母的差异。该功能并不是正则表达语言的一部分，却是许多工具软件提供的有用的相关特征。 `-i`表示忽略大小写的匹配，把`-i`写在正则表达式之前

### 单词分界符

如果`egrep`支持‘元字符序列（metasequences）’。`\<` 和`>/`可以用来匹配单词分界的位置，可以把它们想象为单词版本的`^`和`$` ，分别用来匹配单词的开头和结束位置。

`\<` 和`>/`本身并不是元字符，只有当它们与斜线结合的时候，整个序列才具有特殊意义

### 小结

| 元字符                  | 名称                                           | 匹配对象                                                     |
| ----------------------- | ---------------------------------------------- | ------------------------------------------------------------ |
| . <br> [...]<br>[ ^...] | 点号<br>字符组<br>排除型字符组                 | 单个任意字符<br>列出的任意的字符<br>未列出的任意字符         |
| ^<br>$<br>\\<<br>\\>    | 脱字符<br>美元符<br>反斜号-小于<br>反斜号-大于 | 行的起始位置<br>行的结束位置<br>单词的起始位置<br>单词的结束位置 |
| \|<br>(...)             | 竖线<br>括号                                   | 匹配分隔两边的任意一个表达式<br>限制竖线的作用范围           |

另外还有几点要注意：

- 在字符组内部，元字符的定义规则是不一样的。`^`和`.`、`-`、`^`
- 不要混淆多选项和字符组。字符组`abc`和多选项`(a|b|c)`固然表示同一个意思，但是无论列出的字符有多少，字符组只能匹配一个字符。相反，多选项可匹配任意长度的文本，每个多选项可能匹配的文本都是独立的
- 排除型字符组是表示所有未列出的字符的字符组的简便写法。因此`^x`的意思并不是‘只有当这个位置不是x时才能匹配’，而是说‘匹配一个不等于x的字符’。
- `-i`参数规定在匹配时不区分大小写

### 可选项元素

`？`代表可选项，把它加在一个字符的后面，就代表此处容许出现这个字符，不过它的出现并非匹配成功的必要条件

| 正则表达式            | 描述                        |
| --------------------- | --------------------------- |
| July?(fourth\|4(th)?) | (July\|Jul)(fourth\|4th\|4) |

### 其他量词：重复出现

`+`表示‘之前紧邻的元素出现一次或多次’

`*`表示‘之前紧邻的元素出现任意多次，或者不出现’

`  ...*`表示匹配尽可能多的次数，但如果连一次匹配都没有成功，就报告失败

问号、加号、星号这个三个元字符，统称为量词（quantifiers），因为它们限定了所作用元素的匹配次数

与`？`一样，正则表达式`*`也是永远不会匹配失败的，区别只在于它们的匹配结果，而`+`在无法进行任何一次匹配时，都会报告匹配失败

| 正则表达式 | 描述                       | 例子                                    |
| ---------- | -------------------------- | --------------------------------------- |
| `空格+？`  | 能够匹配一个可能出现的空格 | 空格+任意字符                           |
| `空格+*`   | 能够匹配任意多个空格       | <H[1-6] *>可以匹配\<H1>或者\<H1   >等等 |
| [0-9]+     | 匹配多个相连的数字         | 3213 32321 43242                        |

#### ’表示重复的元字符‘含义小结

|      | 次数下限 | 次数上限 | 含义                                         |
| ---- | -------- | -------- | -------------------------------------------- |
| ？   | 无       | 1        | 可以不出现，也可以只出现一次（单次可选）     |
| *    | 无       | 无       | 可以出现无数次，也可以不出现（任意次数均可） |
| +    | 1        | 无       | 可以出现无数次，但至少要出现一次（至少一次） |

#### 规定重现次数的范围：区间

某些版本的`egrep`能够使用元字符序列来自定义重现次数的区间：`...{min,max}`。这称为‘区间量词（interval quantifier）’。

| 正则表达式    | 描述                              |
| ------------- | --------------------------------- |
| ...{3,12}     | 能够容忍的重复出现次数在3到12之间 |
| [a-zA-Z]{1,5} | 匹配美国股票代码（1-5个英文字母） |

### 括号与反向引用

目前我们见过括号的两种用途：限制多选项的范围，将若干字符组合成一个单元，受问号或者星号之类的量词的作用。

反向引用是正则表达式的特征之一，它容许我们匹配与表达式先前部分匹配的同样的文本。

如果我们确切知道重复单词的第一个单词，比如这个单词为`the`,就能明确无误地找到它。例如`the the`这样或许还会匹配到 `the theory`这种情况。

| 正则表达式         | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| ([A-Za-z]{3}) +\1  | 匹配三个单词的任意字母，并以其为标准匹配下面的单词 (the the the theory the the -> the the,the the)，可以找出重复的相邻单词 |
| ([a-z])([0-9])\1\2 | \1代表[a-z]匹配的内容，而\2代表[0-9]匹配的内容               |

### 神奇的转义

如果需要匹配的某个字符本身就是元字符，正则表达式可以通过加反斜号（backslash）的组合来转义。例如`ega\.att\.com`这样的方法适用于所有的元字符，不过在字符组内无效。

这样使用的反斜号称为‘转移符（escape）’——它作用的元字符会失去特殊含义，成了普通字符。

| 正则表达式       | 描述                 | 例子   |
| ---------------- | -------------------- | ------ |
| \\( [a-zA-Z]+\\) | 匹配一个括号中的单词 | (very) |

## 基础知识扩展

### 更多的例子

#### 变量名

许多程序设计语言都有标识符（identifier,例如变量名）的概念，标识符只包含字母、数字以及下画线，但不能以数字开头。我们可以用`[a-zA-Z][a-zA-a_0-9]*`来匹配标识符。第一个字符组匹配可能出现的第一个字符，第二个（包含对应的`*`）匹配余下的字符。如果标志符的长度有限制，例如最长只能限制是32个字符，可以使用区间量词`{min, max}`，来替换`*`

#### 引号内的字符串

匹配引号内的字符串最简单的办法是使用这个表达式：`"[^"]*"`

两端的引号用来匹配字符串开头和结尾的引号。在这两个引号之间的文本可以包括双引号之外的任何字符。所以我们用`[^"]`来匹配除双引号之外的任何字符，用`*`来表示两个双引号之间可以存在任意数目的为非双引号字符

关于引号字符串，更有用的定义是，两段的双引号之间可以出现由反斜线转义的双引号。

#### 美元金额（可能包含小数）

`\$[0-9]+(\.[0-9][0-9])?`是一种匹配美元金额的办法。

可以大致理解为：一个美元符号，然后是一组字符，最后可能还有另一组字符。这里的字符指的是数字，另一组字符是由一个小数点和两位数字构成的

从几个方面看，上面这个表达式，还是很简陋的，它只能接受$1000,而无法接受\$1,000

#### HTTP/HTML URL

Web URL的形式有很多种，构造一个能够匹配所有形式的URL的正则表达式颇有难度，不过，稍微降低一点要求的话，可以用一个相当简单的正则表达式来匹配大多数常见的URL。常见的 HTTP/HTML URL 如下：

*`http://hostname/path.html`*

初步尝试的正则表达式：

`http://[-a-z0-9_.:]+/[-a-z0-9_:,.!/~*%$]*\.html?`

#### 表示时刻的文字，例如“9：17am”或者“12:30pm”

`(1[012]|[1-9]):[0-5][0-9] (a|p)m`

那么如果是24小时制的时间呢？

`(0?[0-9]|1[0-9]|2[0-3]):[0-5][0-9]`合并为`([01]?[0-9]|2[0-3]):[0-5][0-9]`

### 正则表达式术语汇总

#### 正则（regex）

正则表达式（regular expression）这个全名念起来有点麻烦，更倾向于用正则（regex）来替代

#### 匹配（matching）

指的是这个正则表达式能在字符串中找到匹配文本

#### 元字符（metacharacter）

一个字符是否元字符（或者是“元字符序列”（metasequence），这两个概念是相等的），取决于应用的具体情况。

#### 子表达式（subexpression）

“子表达式”指的是整个正则表达式的一部分，通常是括号内的表达式，或者是由`|`分隔的多选分支。

#### 字符（character）

"字符"在计算机领域是一个具有特殊意义的单词，一个字节所代表的单词取决于计算机如何解释。单个字节的值不会发生变化，但这个值所代表的字符却是由解释所用的编码来决定的。

### 总结

#### 匹配单个字符的元字符

|          | 元字符       | 匹配对象                                                     |
| -------- | ------------ | ------------------------------------------------------------ |
| `.`      | 点号         | 匹配单个任意字符                                             |
| `[...]`  | 字符组       | 匹配单个列出的字符                                           |
| `[^...]` | 排除型字符组 | 匹配单个未列出的字符                                         |
| \char    | 转义字符     | 若char是元字符，或转义字符无特殊含义时，匹配char对于的普通字符 |

#### 提供计数功能的元字符

|             | 元字符   | 匹配对象                           |
| ----------- | -------- | ---------------------------------- |
| `?`         | 问号     | 容许匹配一次，但非必须             |
| `*`         | 星号     | 可以匹配任意多次，也可能不匹配     |
| `+`         | 加号     | 至少需要匹配一次，至多可能任意多次 |
| `{min,max}` | 区间量词 | 至少需要min次，至多容许max次       |

#### 匹配位置的元字符

|      | 元字符     | 匹配对象           |
| ---- | ---------- | ------------------ |
| `^`  | 脱字符     | 匹配一行的开头位置 |
| `$`  | 美元符     | 匹配一行的结束位置 |
| `\<` | 单词分界符 | 匹配一行的开头位置 |
| `>\` | 单词分界符 | 匹配一行的结束位置 |

#### 其他元字符

|         | 元字符      | 匹配对象                                                     |
| ------- | ----------- | ------------------------------------------------------------ |
| `|`     | alternation | 匹配任意分隔的表达式                                         |
| `(...)` | 括号        | 限定多选结构的范围，标注量词作用的元素，为反向引用“捕获”文本 |
| `\1,\2` | 反向引用    | 匹配之前的第一、第二组括号内的字表达式匹配的文本             |

- 转义有3种情况
  - `\`加上元字符，表示匹配元字符所使用的普通字符（例如`\*`匹配普通的星号）
  - `\`加上非元字符，组成一种由具体实现方式规定其意义的院子符序列（例如`\<`表示“单词的起始边界”）
  - `\`加上任意其他字符，默认匹配的就是此字符（也就是说反斜线被忽略了）
- 由星号和问号限定的对象在匹配成功时可能没有匹配任何字符，即使什么字符都不能匹配到，它们仍然会显示匹配成功



[下一章](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md)  

------

