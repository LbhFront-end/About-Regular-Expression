1. [关于这些例子](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#关于这些例子)  
   (1)[Perl简单入门](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#perl简单入门)  
2. [使用正则表达式匹配文本](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#使用正则表达式匹配文本)  
   (1)[向更实用的程序前进](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#向更实用的程序前进)  
   (2)[成功匹配的副作用](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#成功匹配的副作用)  
   (3)[错综复杂的正则表达式](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#错综复杂的正则表达式)  
   (4)[暂停片刻](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#暂停片刻)  
3. [使用正则表达式修改文本](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#使用正则表达式修改文本)  
   (1)[例子：公函生成程序](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#例子公函生成程序)  
   (2)[例子：修正数字的精度](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#例子修正数字的精度)  
   (3)[自动的编辑操作](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#自动的编辑操作)   
   (4)[用环视功能为数值添加逗号](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#用环视功能为数值添加逗号)  
   (5)[Text-to-HTML转换](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part2.md#text-to-html转换)   


## 精通正则表达式第三版（笔记）第二章



## 入门示例扩展

## 关于这些例子

### Perl简单入门

Perl是一门功能强大的脚本语言，Perl关于处理和正则表达式的许多概念来自两种专业化的语言awk和sed，它们都非常不用于‘传统’的语言。

简单的例子

```perl
$celsius = 30;
$fahrenheit = ($celsius * 9 / 5) + 32; #计算华氏温度
print "$celsius C is $fahrenheit" F.\n; #返回摄氏和华氏温度
```

执行这段程序，结果是

```perl
30 C is 86 F
```

`$celsius`和 `$fahrenheit`之类的普通变量一般是以 `$`开头，可以保存一个数值或者任意长度的文本（本例子中只保存了数值）

Perl也提供跟其他流行语言类似的控制结构

```perl
$celsius = 20;
while($celsius <= 45)
{
    $fahrenheit = ($celsius * 9 / 5) + 32; #计算华氏温度
	print "$celsius C is $fahrenheit" F.\n; #返回摄氏和华氏温度
	$celsius = $celsius + 5;
}

```

## 使用正则表达式匹配文本

Perl可以多种方式使用正则表达式，最简单的就是坚持变量中的文本能否由某个正则表达式匹配。下面的代码检查 `$reply`中包含的字符串，报告这个字符串是否全部由数字构成

```perl
$reply = "12";
if($reply =~ m/^[0-9]+$/){
  print "只有数字\n"
}else{
  print "不只有数字\n"
}
```

上述代码中，`=~`代表匹配，`m`可有可无， `^...$`保证了整个变量只包含数字，乳沟去掉则是表示只要变量中包含任意的数字字符。

结合上面的例子，可以提醒用户输入一个值，然后接受这个输入，用一个正则表达式来验证，确保输入的是一个数字，如果是，我们就计算对应的华氏温度，否则就会输出一条报警消息

```perl
print "请输入一个摄氏温度： \n";
$celsius = <STDIN>;
chomp($celsius);
if($celsius =~ m/^[0-9]+$/){
  $fahrenheit = ($celsius * 9 / 5) + 32;
  print "$celsius C is $fahrenheit F\n";
}else{
  print "$celsius 不单单只有数字 \n";
}
```

`printf`格式化输出，可以解决一些数字精度的问题，不会改变变量的值，而只是改变显示的方式

### 向更实用的程序前进

扩展这个例子，容许输入负数和可能竖线的小数部分，需要修改正则表达式。

`[-+]?`来处理开头的正负号

`（\.[0-9]*）？`来处理小数点

结合起来就得到

`/^[-+]?[0-9]$/`

那么条件判断语句：

```perl
if($celsius =~ m/^[-+]?[0-9]+(\.[0-9]*)?$/){}
```

它能够匹配32、-3.3232、+98.6这样的文字。不过还不够完善，它不能匹配以小数点开头的数（例如.257）。

### 成功匹配的副作用

让这个表大四能够匹配摄氏和华氏温度，我们让用户再温度的末尾加上C或者F来表示。我们可以在正则表达式末尾加上`CF`来匹配用户的输入。

```perl
print "请输入一个摄氏温度： \n";
$input = <STDIN>;
chomp($input);
if($input =~ m/^([-+]?[0-9]+)([CF])$/){
  $InputNum = $1;
  $type = $2;
  if($type eq "C"){
    $celsius = $InputNum;
    $fahrenheit = ($celsius * 9 / 5) + 32;

  }else{
    $fahrenheit = $InputNum;    
    $celsius = ($fahrenheit -32) * 5 / 9;    
  }
  printf "%2f C is %2f F\n", $celsius, $fahrenheit;
}else{
  print "输入的数字后面应该有单位（C或者F） \n";
  print "无法理解 $input \n";
}

```

### 错综复杂的正则表达式

因为 `（\.[0-9]*）？`可以处理浮点数，那么在上面例子基础上再增加能够匹配浮点数的要求，可以得到

```perl
if( $input  =~ m/^([-+]?[0-9]+(\.[0-9]*)?)([CF])$/)
```

接下来处理数字和字母之间可能出现的空格，我们知道，正则表达式中的空格符证号对应匹配文本中的空格字符，所以 `空格+*`可以匹配任意数目的空格（但不是必须出现空格），所以得出

```perl
if( $input =~ m/^([-+]?[0-9]+(\.[0-9]*)?) *([CF])$/)
```

制表符`tab`也会产生空白，上述 `空格+*`可以改成 `[空格\t]*`

\`\b`,通常是匹配一个单词的分界符，但是在字符组中，它匹配一个退格符。单词分界符作为字符组的一部分则没有意义，所以Perl完全可以用它来匹配其他的字符。

#### 用\s匹配所有“空白”

我们刚刚最后使用的是 `[空格\t]*`。这样做没有问题，但是很多流派的正则表达式提供了一种更方便的方法，那就是 \`\s`，能表示所有表示“空表字符（whitespace character）”的字符组，其中包括空白符，制表符，换行符和回车符。

代码优化为

```perl
if( $input =~ m/^([-+]?[0-9]+(\.[0-9]*)?)\s*([CF])$/)
```

另外还有温度制式的大小写字母，可以做以下忽略大小写优化

```perl
if( $input =~ m/^([-+]?[0-9]+(\.[0-9]*)?)\s*([CF])$/i)
```

添加的这个 `i`叫做“修饰符”，把它放在 `m/.../`结构之后，就是代表着进行不区分大小写的匹配。修饰符不是正则表达式的一部分，而是 `m/.../`结构的一部分。另外常见的还有 `/g`表示“全局匹配”以及 `/x`表示宽松排列的表达式

温度转换程序的最终版本

```perl
print "请输入一个摄氏温度： \n";
$input = <STDIN>;
chomp($input);
if($input =~ m/^([-+]?[0-9]+(\.[0-9]*)?)\s*([CF])$/i){
  $InputNum = $1;
  $type = $3;
  if($type =~ m/c/i){
    $celsius = $InputNum;
    $fahrenheit = ($celsius * 9 / 5) + 32;

  }else{
    $fahrenheit = $InputNum;    
    $celsius = ($fahrenheit -32) * 5 / 9;    
  }
  printf "%2f C is %2f F\n", $celsius, $fahrenheit;
}else{
  print "输入的数字后面应该有单位（C或者F） \n";
  print "无法理解 $input \n";
}

```

### 暂停片刻

1. Perl用 `$variable =~ m/regex/`来判断一个正则表达式是否能匹配某个字符串，`m`表示“匹配(match)”,而斜线用来标注正则表达式的便捷（它们本身不属于正则表达式）。整个测试语句作为一个单元，返回true或者false值

2. 元字符——具有特殊意义的字符——的定义在正则表达式中并不是统一的。元字符的含义取决于具体的情况。

3. Prel和其他流派的正则表达式提供了许多有用的简记法（shorthands）

   | 简记符 | 描述 |
   | ------ | ---- |
   | `\t` | 制表符 |
   | `\n` | 换行符 |
   | `\r` | 回车符 |
   | `\s` | 任何“空白”字符（例如空格符、制表符等） |
   | `\S` | 除 `\s`之外的任何字符 |
   | `\w` | `[a-zA-Z0-9]`(在`\w` +)中很有用，可以用来匹配一个单词 |
   | `\W` | 除`\w`之外的任何字符，也就是 `[^a-zA-Z0-9]` |
   | `\d`   | `[0-9]`，即是数字 |
   | `\D`   | 除`\d`之外的任何字符，即 `[^0-9]` |

4.  `/i`修饰符表示此测试仪不区分大小写。尽管写法 是 “/i”，但其实 “i”只是更在表示结尾的斜线之后

5. `(?:...)`这个写法可以用来分组文本，但不获取

6. 匹配成功之后，Perl可以用 `$1`、`$2`、`$3`之类的变量来保存相对应 `(...)`括号内的子表达式的文本。使用这些变量，可以哟女正则表达式从字符串中提取信息。子表达式的编号按照开括号的出现先后排序，从1开始。子表达式可以嵌套。如果只是希望分组，也可以用 `(...)`，但是副作用就是它们会捕获文本，保存在特殊的变量中

## 使用正则表达式修改文本

Perl和许多其他语言提供的一个正则表达式的特征：替换（substitution, 也可以叫做“查找与替换”（search and replace））。

`$var  =~ m/regex`用正则表达式来匹配保存在变量中的文本，并返回表示能够匹配的布尔值。

`$var =~ s/regex/replacement`则是如果正则表达式能够匹配 `$var`中的某段文本，就将这段匹配的文本替换为`replacement`。其中 `regex`与之前的 `m/.../`用法是一样的，而 `replacement`（位于第二个和第三个斜线之间）则是作为双引号内的字符串。这就是说，在其中可以使用变量来引用之前的匹配的具体文本。

所以使用 `$var =~ s/.../.../`可以改变 `$var`中文本（如果没有找到匹配的文本，也就不会有替换的发生）。例如：结果变量中包含 `Jeff Friedl`,运行下列代码

```perl
$var =~ s/Jeff/Jeffrey/;
```

`$var`的值也就变成了 `Jeffrey Friedl`。但是如果再运行一次，就变成了 `Jeffreyrey Friedl`。要避免这种情况的发生，我们需要添加表示单词分界的元字符。Perl提供统一的元字符 `\b`来代表“单词起始”和“单词结束”。与 `m/.../`一样 `s/.../...`也可以使用修饰符

### 例子：公函生成程序

假设有一个公函系统，它包含很多公函模板，其中有一些标记，对每一封具体的公函来说，表示部分的值都有所不同

例子

```perl
亲爱的 =FIRST=,
您已经被我们选中，可以免费领取价值 =PRICE= 的 =TRINKET=，=TOTAL= 你值得拥有
```

假设变量的值为

```perl
$given = "陈先生"；
$price = "1000万"；
$prize = "钻石"；
```

准备好之后，既可以用下面的语句进行填写模板

```perl
$letter =~ s/=FIRST=/$given/g;
$letter =~ s/=PRICE=/$price/g;
$letter =~ s/=TRINKET=/$prize/g;
$letter =~ s/=TOTAL=/$price的$prize/g;
```

### 例子：修正数字的精度

有时候，我们在操作数字运算的时候，会得到“9.0500000024456”类似这样的数字。本来我们想拿到的数字应该是“9.05”,这是因为计算机内部表示浮点的原理，我们可以用 `printf`来保证只输出两位小数，但是如果遇到 “1/8”的数字，则应该输出3位小数点。通过分析可以得知，通常是保留小数点后两位数字，如果第三位不为0，也需要保留，去掉其他数字。用一下正则

```perl
$price =~ s/(\.\d\d[1-9]?)\d*/$1/;
```

上述正则中， `\.`匹配小数点，接下来的 `\d\d`匹配开头的两位数字。 `[1-9]?` 匹配可能更在后面的非零数字。然后用 `()`将它保存在 `$1`来放在替换成的文本的位置，接着在`()`后面用 `\d*`表示其他多余的文本。那么后面的 `$1`就会替换掉前面的文本，被删除的文本就是其他多余的数字。

### 自动的编辑操作

将文本汇总所有出现的 `sysread`改成 `read`。

```perl
perl -p -i -e 's/syread/read/g' file
```

参数 `-e`表示整个程序接在命令的后面，参数 `-p`表示对目标文本的每一步进行查找和替换，而 `-i`表示将替换的结果写回文件。

注意，这里没有明确写出查找和替换的目标字符串（也就是说没有 `$var =~ '''`）因为 `-p`参数就表示对目标文件的每行文本应用这段程序，同时使用了 `-g`这个修饰符，就可以保证在一行文本中可以进行多次替换

尽管上例只是对一个文件进行操作，但是也容易在命令行中列出多个文件，那么就可以把替换命令应用到每一行文字。这样只需要一行简单的命令，就可以编辑大量的文件。

匹配邮件的发送地址：

```perl
target:
From: 3213213@da232.org (The King)

实现：
^From: (\S+) \(([^()]*)\)

解释：
[^()]* 表示出 ()"括号"之外的任何字符
\S+ 表示第一个空白之前的文本或者目标文本末尾之前的所有字符
```

### 用环视功能为数值添加逗号

“4456456465”一般写成“4,456,456,465”会比较容易算，从右到左很容易就可以得出结论，可视正则表达式都是从左到右工作的。梳理一下发现，逗号应该加在“左边有数字，右边的数字个数正好是3的倍速的位置”，这样，使用一组相对较新的正则表达式特性——“环视（lookaround）”可以轻松解决这个问题。

环视结构不匹配任何字符，只匹配文本中的**特定位置**，这点与单词分界符 `\b`和锚点 `^`和 `$`很相似。但是，环视比它们更加通用。

一种类型的环视叫做“顺序环视（lookahead）”,作为表达式的一部分，代表从左到右查看文本，尝试匹配子表达式，匹配成功就会返回匹配成功消息。肯定型顺序环视（positive lookahead）用特殊的序列 `(?=...)`来表示，例如 `(?=\d)`，它表示如果当前的位置的右边的字符是数字则匹配成功。另一种为逆序环视，从右到左查看文本，用特殊的序列 `(?<=...)`来表示。例如 `(?<=\d)`,如果当前位置的左边有一位数字，则匹配成功，（也就是说，紧跟在数字后面的位置）

#### 环视不会“占用”字符

即匹配但是不会捕获

```perl
$pop =~ s/(?<=\d)(?=(\d\d\d)+$)/,/g;
```

#### 四种类型的环视

| 类型                         | 正则表达式               | 匹配成功的条件...                                    |
| ---------------------------- | ------------------------ | ---------------------------------------------------- |
| 肯定逆序环视<br>否定逆序环视 | `(?<=...)`<br>`(?<!...)` | 子表达式能够匹配左侧文本<br>子表达式不能匹配左侧文本 |
| 肯定顺序环视<br>否定顺序环视 | `(?=...)`<br>`(?!...)`   | 子表达式能够匹配右侧文本<br>子表达式不能匹配右侧文本 |

那么假如我们的数字是在一段文本中的，我们的正则表达式可以修改为

```perl
$text =~ s/(?<=\d)(?=(\d\d\d)+(?!\d))/,/g;
```

### Text-to-HTML转换

变量的为多行文本，需要考虑到处理采用换行符作为一行的终结符之外，还有使用回车/换行的结合体，确保程序可以应对这两种情况

#### 处理特殊的文字

原始文本会包含“&”，“<”，">"，转换为对应的HTML编码，分别是“&amp”、“&lt”、“&gt”。在HTML中这些字符有特殊的含义，编码不正确可能会导致显示错误。

```perl
$text =~ s/&/&amp;/g;
$text =~ s/</&lt;/g;
$text =~ s/>/&gt;/g;
```

#### 分隔段落

我们用HTML 的p标签来标记段落。识别段落的简单办法就是把空行作为段落之间的分隔。最容易想起的方法是

```perl
$text =~ s/^$/<p>/g;
```

它可以匹配“行末尾随行开头的位置”，但是 `^`和 `$`通常匹配的不是逻辑行的开头和结尾，而是真个字符串的开头和结束位置。所以，既然目标字符串中有多个逻辑行，就需要采取不同的办法。

大多数支持正则表示的语言提供了一个简单的办法，就是“增强的行锚点（enhanced line anchor）”匹配模式，在这种模式下， `^`和 `$`会从字符串模式切换到本例子中需要的逻辑行模式。在Perl，使用 `/m`修饰符来选中此模式

```perl
$text =~ s/^S/<p>/mg;
```

`^\s*$`表示“寻找连续、空行和只包括空白字符的行的结合”

`^[ \t\r]*$`表“寻找空行以及包括空白字符的行”

#### 将E-mail地址转换为超链接形式

识别邮箱地址，转换为“mailto”链接。例如

```perl
fed@qq.com -> <a href="mailto:fed.qq.com"> fed.qq.com </a>
```

大致思路

```perl
$text =~ s/\b(username regex\@hostname regex)\b/<a href="mailto:$1">$1<\/a>/g;
```

#### 匹配用户名和主机名

匹配邮箱地址最简便的方法是 `\w+\@\w+(\.\w+)+`,实际应用起来的话，我们需要考虑到更周到一些，用户名可以包含点号和连字符（虽然用户名不会以这两种字符开头）。可以用 `\w[-.\w]*`来替换 `\w`。而主机名的匹配则要匹配则要复杂一点，因为点号只能作为分隔符，也就是说两个句号之间必须有其他字符。

```perl
\@[-a-z0-9]+(\.[-a-z0-9]+)*\.(com|edu|info)
```
[上一章](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part1.md) 
[下一章](https://github.com/LbhFront-end/About-Regular-Expression/blob/master/About-Regular-Expression-Part3.md)  
