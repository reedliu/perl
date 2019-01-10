# 为什么要学编程？

> 刘小泽写于19.1.8
>
> 其实之前我也一直困惑这个问题？“为什么一定要自己学习编程，那么多写好的工具，直接拿来用不是更方便吗？我们毕竟是攻城狮而不是程序猿。”我如是安慰自己。
>
> 但所有的事情都有一样的规律，现在认为不重要，只是因为还没到那个程度。的确，开始我也想学编程python、perl的书也准备了，但却一直用不上知识，于是就产生了间断期。最近，有强烈的需求想要自己去探索数据的时候，却有点摸不着头脑了，因为现成的工具并不能满足我的全部需求，而且大脑里已经想象出了怎么去一步步处理，就是做不出来。这一次，我真的好想学习一下编程技术，不要求造多好的轮子，只要求自己用到的时候，有想法，能实现，就够了。之后的事情，就要等到再进阶，再有需求的情况去做了。

### Perl和Python之间，我先选择Perl

这二者都很好，但是精力有限，只想先看懂简单的代码流程，另外perl one-line命令一直吸引着我，想借此机会探究一下，伊现富老师（https://github.com/Yixf-Education）翻译过一本Perl的书籍《Perl语言在生物信息学中的应用—基础篇》。今天先学习了三分之一，记录下个人认为重点的一些地方，里面有许多知识需要不断揣摩

### 什么是编程语言？

指定一系列语法规则指导计算机运行，编写的程序叫源代码，需要翻译成机器语言才能让计算机识别运行。机器语言是二进制，难以肉眼读取编写，因此还需要解释器（编译器）才能运行某种编程语言写的程序（如perl解释器运行perl程序）

### 编程过程

> 例如：我们手头有一个DNA序列想看下其中的调控元件数目，大脑中的想法框架应该这样构建：

- **确定输入**--新的DNA序列

  问题：输入文件名=》文件是否存在=》打开文件

- **确定查询**—调控元件列表

  就像DNA序列一样也要添加进来

- **进行查找**--算法

  一般会有好几种解决办法：

  算法一：读入一个调控元件，从头到尾对DNA查找，然后进行下一个调控元件；

  算法二：一次性读取DNA序列，再在DNA的每一个位置对调控元件进行查找

- 输出

#### 伪代码帮记录

> 灵感最重要，先将整体思路用语言先写出来。还是上面例子，我们可以这样写

```perl
get the name of DNA file

read in the DNA file

for each regulatory element
	if element in DNA, then
		add one to the count
print count
```

之后伪代码可以变成注释信息，方便自己一步步去写需要的代码，并且方便交流

---

### 常用元素

- 命令解释

  第一行#起始：`#! /usr/bin/perl -w` ，告诉计算机这个是perl程序。-w表示遇到错误发出警告

- 语句

  分号结尾，如赋值语句`$DNA='ATCGCTACG';` ，将右边的真实DNA信息存储在perl程序中的DNA变量中

- 变量

  只由大小写字母、数字、下划线组成，不能数字开头

  `$`开头的叫标量变量，储存字符串或各种数字，但是一个标量一次只能存储一种类型

- 换行

  `print $DNA \n\n`表示输出DNA后中间空一行再打印其他

- 单引号与双引号

  单引号属于强引用，其中的任何东西都会输出为字符，例如：`"\n"`会打印输出换行符，`'\n'`只会打印\n

- 文件句柄

  就是处理文件需要用的东西，例如`$protein=<PROTEIN>`中的PROTEIN

  大小写没有规定，但一般使用大写表示；

  其中尖括号表示输入操作符，就是利用这个`<PROTEIN>`从外部读入数据，一般操作是：

  ```perl
  $proteinfilename = 'NM_021964fragment.pep';
  open( PROTEINFILE, $proteinfilename );
  # 然后就可以把读入的PROTEINFILE存到数组中
  ```

  常用的句柄还有：`STDIN`（从键盘上读取用户的输入）；`STDOUT` 把输出内容输出到屏幕（默认）或到其他位置；错误信息定向到`STDERR` ，例如：`print STDERR "Bad info\n";` 

- 数组

  `@`开头，是储存多个标量的变量

  ```perl
  @bases = ('A', 'C', 'G', 'T');
  print @bases; #输出结果没空格
  print "@bases"; #输出结果有空格
  ```

- `pop` 数组末尾拿掉一个元素；`shift`开头拿掉一个元素；
  `unshift`添加一个元素到数组开头；`push `添加一个元素到数组结尾

- Perl中有三种主要数据类型：标量变量、数组和散列（关联数组）。散列以%起始，如`my %dictionary = (...);`。取元素时，散列和数组一样用`$`，因为查找返回的都是标量值，散列与数组不同之处就是：散列用`{}`，数组用`[]` 

  获取散列的所有键构成的数组：`@keys = keys %my_hash;` ；获取散列所有值构成的数组：`@values = values %my_hash;` 利用键值对可以快速提取信息，例如基因名作为键，基因相关信息作为值【注意：不能把正则表达式或字符集作为散列的键，必须是简单的标量值，比如一个字符串或一个数字】

  对散列进行排序：`@sorted_keys = sort keys %my_hash;` 

- 括号：在复杂的运算中指明运算的顺序

---

### 有用的操作

#### 连接

- 使用`.`，`$DNA3=$DNA1.$DNA2` ，然后`print "$DNA3\n";` 
- `$DNA="$DNA1DNA2"`，然后`print "$DNA3\n";` 
- `print $DNA1, $DNA2, "\n";`或`print "$DNA1$DNA2\n";` 
- `$protein=join('',@protein)`，需要指定一个字符串，会放在数组各个元素之间，当分隔字符串为空字符串时，就将各个元素连起来

#### 替换--s///

![image.png](https://upload-images.jianshu.io/upload_images/9376801-9f8e44cfc6d5ca75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意到`=~`，它是绑定操作符，就是将右边的操作应用到左边变量的字符串上；

然后`s/T/U/g`，s表示替换（substitution），第一个斜线后跟着T，表示字符串中被替换的元素；第二个斜线后跟着U，是要替换成的元素；g表示全局

因此，这句命令的意思就是：把$RNA变量存储的字符串数据中所有T变成U

另外，如果要删除换行符或者空格或制表符，使用`$RNA =~ s/\s//g` 

#### 反转数组--reverse

`reverse @bases`   ，例如：

```perl
@bases = ('A', 'C', 'G', 'T');
@reverse = reverse @bases;
print "@reverse\n\n";
# T G C A
```

#### 互补--tr

`tr` 其实也是替换的作用，但是对于求序列的互补比较方便，第一个斜线后是要被替换的内容；第二个斜线后是替换后的内容，且位置对应进行替换

![image.png](https://upload-images.jianshu.io/upload_images/9376801-3ad04e71068d3b8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为tr函数会返回在字符串中找到特定字符的数目，而且即使替换字符集为空，原始字符集也不会改变，因此还可以作为**计数器**，如：`$base = ($dna =~ tr/ACGTacgt//);`就可以统计所有碱基数

#### 获取长度--scalar

`scalar @bases` ，对于上面的例子，得到的结果就是4

#### 任意位置插入--splice

`splice` ，例如：

```perl
@bases = ('A', 'C', 'G', 'T');
splice ( @bases, 2, 0, 'X' );
print "@bases\n";
# A C X G T
```

参考：http://blog.sina.com.cn/s/blog_4d3a41f40100m29m.html

#### 拆解--split

`@DNA=split('',$DNA)` 将$DNA拆解成单个字符的@DNA数组【与join相反】

#### 取子集--substr

`$base = substr($DNA, $position,1)` 

#### 变量初始化

声明变量后需要赋一个值，如果不进行初始化，它的值将被定义为'`undef`'，对于undef，在数字操作中表示0，在字符串操作中表示空字符串

#### 循环

- foreach循环

  ```perl
  # 统计碱基数目
  foreach $base (@DNA){
      $count++; #效果等同于$count += 1
  }
  ```

  当然，如果不指定从数组中读取的标量变量，如`foreach (@DNA)`，perl会使用特殊变量`$_` 

- while循环

  ```perl
  while ($DNA =~ /a/ig) {$a++} #统计A碱基的数目(i表示不区分大小写)
  ```

- for循环示例

  ```perl
  # DNA翻译成蛋白序列(codon2aa是一个子程序，记录了密码子与氨基酸的对应)
  for (my $i=0; $i<(length($dna)-2);$i += 3){
      $protein .= condon2aa (substr($dna, $i, 3));
  }
  return $protein;
  ```

#### 排序

- 按字母顺序升序：`@array = sort @array;`
- 按数字升序：`@array = sort {$a <=> $b } @array;` 

#### 范围操作符

`(1../comments/) and next;` ，其中`(..)` 是范围操作符，表示跳过第一行到包含comments的所有行。然后`and`是一个逻辑操作符，表示跳过后继续进行

#### 逻辑操作符

`and`可以用`&&`表示，`or` 可以用`||` 表示，`not`用`!`表示

#### 快速调用匹配值

```perl
# 例如：'123456' =~ /34/ 可以匹配成功
$&就表示匹配上的内容'34'
$`表示匹配成功前面的内容'12'
$'表示匹配成功后面的内容'56'
```

#### 正则表达

要想在正则表达式中体现`//`，可以用`/\/\/\n/` ，但总感觉有点奇怪，像这种情况，可以在前面使用m，利用其它的界定符（一般我们使用`/`，但并不代表只有这一种界定方式），从而避免在正斜线前使用转义符，如`m!//\n!`  

**看一个例子**：`while( $features =~ /^ {5}\S.*\n(^ {21}\S.*\n)*/gm ) {};` 

这是while循环中添加的一个正则表达式匹配：如果features满足一定的要求，就执行while语句下面的内容，那么满足什么要求呢？那就需要看表达式

不要被看似复杂的结构搞晕，直接看`/ /`中间包围的那部分：其中`\n`表示换行符，那我们先看第一个`\n`前面的内容：
 `^ {5}\S.*\n` 表示以5个空格开头`^ {5}`，然后跟着一个非空白字符`\S`，再接任意个非换行符字符`.*`

然后第二个`\n`前面的是`(^ {21}\S.*\n)*` 表示 以21个空格开头，然后加一个非空白字符，再接任意个非换行字符。并且注意到这部分内容被小括号包围，然后跟了星号，表示小括号中的内容可有可无。

**因此，`/ /`内部整体上说明的是：** 匹配第一行以5个空格开头，下面的行中以21个空格开头，但这部分可有可无。



#### 模式修饰

常用的**模式修饰符**：用于全局匹配的`/g`，不分区大小写的`/i`，将整个字符串当成单独一行的`/s`（忽略换行符），将整个字符串作为含有换行符的行`/m` 

搭配**元字符**： `^`锚定开头，`$`锚定结尾，`.`匹配除了换行符以外的任意字符

例如：要对`AAC\nGTT` 进行匹配

```perl
use warnings;
"AAC\nGTT" =~ /^.*$/; #使用^.*$可以匹配除了换行符外的任意字符
print $&, "\n";
# 结果报错

# 原因就在于：在中间出现了一个换行符，默认的模式修饰不能识别，因此可以转换成m修饰，会从字符串开头一直匹配到第一个换行符(即便出现在内部)
"AAC\nGTT" =~ /^.*$/m;
print $&, "\n";
#结果输出AAC，$&就是匹配到的内容

# 如果用/s，就可以匹配包括换行符在内的所有
"AAC\nGTT" =~ /^.*$/s;
print $&, "\n";
# AAC
# GTT
```



---

### 子程序

子程序可以重复使用，可以单独进行测试，子程序本身可以调用其他子程序，总而言之，**子程序就是只做一件事并把它做好**，然后我们要做的就是：**组合它们来完成整个任务**

#### 编写子程序示例

子程序一般包括：`sub`、子程序名字、大括号代码块

子程序从`@_` 数组中收集参数值，然后赋给子程序的变量（子程序的变量名可以和主程序重名，并不冲突）。如果只在子程序中写`my ($dna)`而不加`= @_`，那么子程序就会使用主程序的同名变量或者报错找不到新变量

```perl
sub addbase {
    my ($dna) = @_; #perl的特殊数组是“_”。my用来声明一个新变量，只在包裹的代码块中有效
    $dna .= 'ATGC';
    return $dna
}
```

**切记：子程序中定义任何变量，都要使用`my`** ，可以在程序开始添加`use strict;` 提醒我们使用`my`定义变量

例如：计算DNA中G的数目

```perl
#!/usr/bin/perl -w
use strict; #强制声明变量

# 主程序
# 定义变量USAGE作为程序帮助文档，其中包含所有命令行参数[这里包括了$0和DNA]
# $0也是特殊变量，表示这个perl脚本名；DNA指的是需要的DNA序列
my ($USAGE) = "USAGE：$0 DNA\n\n"
# 数组变量@ARGV是内置变量，其中包含输入的参数；如果没有输入参数就打印程序帮助文档USAGE
unless (@ARGV){
    print $USAGE;
    exit;
}
my ($dna) =$ARGV[0]; #提取数组第一个元素，注意要换成$提取(perl的坐标从0开始)
my ($sum_Gs) = countG($dna);
print "\nThe DNA $dna has $sum_Gs G\'s in it!\n\n";
exit;

#子程序
sub countG{
    my ($dna) = @_;
    my ($count) = 0;
    $count = ($dna =~ tr/Gg//);
    return $count;
}
```

#### 传递数据给子程序

- 值传递：（参数比较简单）主程序参数复制后传给子程序，子程序中这部分值改变并不会影响主程序相应参数的值，例如：

  ```perl
  #!/usr/bin/perl -w
  use strict;
  
  my $i=2;
  subvalue($i);
  print "In the main program, the result is $i\n";
  exit;
  
  sub subvalue{
      my ($i)=@_;
      $i += 100;
      print "In the sub program, the result is $i\n";
  }
  
  #In the sub program, the result is 102
  #In the main program, the result is 2
  ```

- 引用传递：（参数比较复杂）可以向子程序中传递标量、数组等各种形式的参数，子程序中所做的任何事都改变主程序中的对应参数。重点是：在引用的变量名前加一个反斜线

  ```perl
  #!/usr/bin/perl -w
  use strict;
  
  #主程序
  my @i = ('1','2','3');
  my @j = ('a','b','c');
  print "Mian-1 program: i = "."@i\n";
  print "Mian-1 program: j = "."@j\n";
  
  subvalue(\@i,\@j);
  print "Main-2 program: i = "."@i\n";
  print "Main-2 program: j = "."@j\n";
  exit;
  
  #子程序
  sub subvalue{
      my($i,$j)=@_;
      print "Sub program: i = "."@i\n";
  	print "Sub program: j = "."@j\n";
  	push(@$i,'4');
  	shift(@$j);
  }
  
  #Mian-1 program: i = 1 2 3
  #Mian-1 program: j = a b c
  #Sub program: i = 1 2 3
  #Sub program: j = a b c
  #Main-2 program: i = 1 2 3 4
  #Main-2 program: j = b c
  ```

#### 重复使用的子程序—模块/库

子程序确实很方便，但是一些比较好的子程序需要应用到许多脚本中，一旦需要修改就需要修改所有脚本中的子程序，这是非常麻烦的。因此，如果一开始将子程序放到一个方便使用的文件中，如果改动就只改一次，然后可以应用到所有的脚本就好了。因此，模块（库）就诞生了。

例如，我们有一个库叫`beginperl.pm`（后缀名是perl module的意思），里面存放了所有的子程序。【需要注意的是，`.pm`文件的最后一行要是`1;`】。

使用库的时候，直接使用`use beginperl;` 【注意：这条语句放在靠近`use strict;`的地方；库的名字不带后缀；库如果和脚本不在一个目录下，需要绝对路径】

---

### 应用

#### 一：提取Genbank序列和注释信息

先对Genbank的信息分布有个大体了解，比如序列信息在最后，从`ORIGIN`开始到最后一行的`//`，注释信息是ORIGIN之前的信息

**基本思路：**加载文件=》遍历数组（文件每一行）=》先匹配`//` 得到最后一行行号=》再匹配`ORIGIN`得到分界线行号【之所以先匹配`//`是因为如果先匹配分界线的话，一旦匹配上就不会再继续进行了，也就得不到最后一行的行号了】=》序列就是ORIGIN行向下，注释就是ORIGIN行向上

```perl
#前提得到Genbank文件@Genbankfile
#sub open_file
#	given the filename, return the filehandle
sub open_file {
    my ($filename) = @_;
    my $fh;
    unless (open($fh, $filename)){
        print "cannot open\n";
        exit;
    }
    return $fh;
}
# 这里只是列一个核心
my $linenumber = 0;
my $end;
my $origin;
foreach my $line (@Genbankfile){
    if ($line =~ /^//\n/) {
        $end = $linenumber;
        last;
    }elsif ($line =~ /^ORIGIN/){
        $origin = $linenumber;
    }
    $linenumber++;
}
@annotation = @Genbankfile[0..($origin-1)];
@seq=@Genbankfile[($origin+1)..($end-1)];
```











**资料：**

http://barc.wi.mit.edu/education/bioinfo2003/perl_bioinfo.html