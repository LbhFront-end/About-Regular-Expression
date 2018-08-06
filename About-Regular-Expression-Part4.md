1. [正则引擎](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part4.md#正则引擎)  
   (1)[ 分类](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part4.md#分类)  
   (2)[测试引擎的类型](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part4.md#测试引擎的类型)  
2. [匹配基础](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part4.md#匹配基础)  
   (1)[规则1：优先选择最左端的匹配结果](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part4.md#规则1优先选择最左端的匹配结果)  
   (2)[引擎的构造](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part4.md#引擎的构造)  
   (3)[规则2：标准量词是匹配优先的](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part4.md#规则2标准量词是匹配优先的)  

## 精通正则表达式第三版（笔记）第四章

## 表达式的匹配原理

## 正则引擎

### 分类

正则引擎粗略地分为3类：

- DFA（符合或不符合POSIX标准的都属于此类）
- 传统型NFA
- POSIX NFA

### 测试引擎的类型

通过忽略优先量词，可以知道NFA支持，DFA不支持，而在POSIX NFA也是没有意义的。用正则表达式 `nfa|nfa not`就可以进行测试。若 结果为 `nfa`就是传统的 NFA类型，如果是 `nfa not`，要么是POSIX NFA，要么是DFA。

#### DFA还是POSIX NFA

DFA不支持捕获型括号（capturing parenthess）和回溯（backreference），这一点有助于判断，不过，也存在同时使用两种引擎的混合系统，在这种系统中，没有使用捕获型括号，就会使用DFA

```
echo =XX============================== | egrep 'X(.+)+X'
```

如果执行时间很长，就是NFA（如果上一项测试显示这不是传统型NFA，那么它肯定是 POSIX NFA）。如果时间很短，就是DFA，或者是支持某些高级优化的NFA。如果显示堆栈超溢（stack overflow）,或者超时退出，那么它就是NFA引擎



## 匹配基础

整个第四章只能列出两条普适的原则：

- 优先选择最左端（最靠开头）的匹配结果
- 标准的匹配量词（`*`、`+`、`?`、`{m,n}`）是优先匹配的

### 规则1：优先选择最左端的匹配结果

根据这条规则，起始位置最靠左的匹配结果总是优于其他可能的匹配结果。这条规则并没有规定优先的匹配结果的长度，而只是规定，在所有可能的匹配结果中，优先选择开始位置的最左端。

这条规则的由来是：匹配先从需要查找的字符串的起始位置尝试匹配，这里，尝试匹配（attempt）的意思是，在当前位置测试了整个正则表达式能匹配的每样文本。如果在当前位置测试了所有的可能之后不能找到匹配结果，就需要从字符串的第二个字符之前的位置开始尝试。在找到匹配结果以前必须在所有的位置重复此过程。只有在尝试过所有的起始位置（直到字符串的最后一个字符）都不能匹配结果的情况下，才会报告‘匹配失败’

### 引擎的构造

正则引擎中零件分为几类——文字字符（literal characters）、量词（qualifilers）、字符组（character classess）、括号，等等。

#### 文字文本（Literal Text） 例如 `a`、`\*`、`!`、枝

对于非元字符的文字字符，尝试匹配时需要考虑就是“这个字符与当前尝试的字符相同吗？”。

#### 字符组、点号、Unicode属性及其他

通常情况下，字符组、编号、Unicode属性以及其他的匹配时比较简单的：无论字符组的长度是多少，它都只能匹配一个字符

点号可以很方便地表示复杂的字符组，它几乎能匹配所有的字符，所以它的作用也很简单

#### 捕获型括号

用于捕获文本的括号（而不是用于分组的括号）不会影响匹配的过程。

#### 锚点（`^`、`\z`、`（?<=\d）`）

锚点可以分成两大类：简单锚点（`^`、`\z`、`$`、`\b`）和复杂锚点（顺序环视和逆序环视等）。简单锚点之所以得名，在于它们只是检查目标字符串中的特定位置的情况，或者是相邻两个字符。想法，复杂锚点能包含任意复杂的子表达式，所以它们也可以任意复杂。

#### 括号、反向引用和忽略优先量词

捕获信号只对NFA起作用。忽略优先量词也一样。



### 规则2：标准量词是匹配优先的

标准匹配量词（`？`、`*`、`+`、`{min, max}`都是“匹配优先（greedy）”的）。标准匹配量词的结果“可能”并非所有可能汇总最长的，但它们总是尝试匹配尽可能长的字符，直到匹配上限为止。如果最终结果并非该表达式的所有可能中最长的，原因肯定是匹配字符过多导致了匹配失败。

匹配优先量词之所以得名，是因为它们总是（或者，至少是尝试）匹配多余匹配成功下限的字符。



[上一章](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part3.md)

[下一章](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part5.md) 