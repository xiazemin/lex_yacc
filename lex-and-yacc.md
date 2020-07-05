从下列地方可以等到lex 和yacc 工具：

> ● Mortice Kern System \(MKS\)，在 www.mks.com，
>
> ● GNU flex 和 bison，在 www.gnu.org ，
>
> ● Ming，在 www.mingw.org，
>
> ● Cygwin，在 www.cygwin.org

“lex”和“yacc”这两个名字所代表的也包括这些工具的 GNU 版本 flex 和 bison

**终端和非终端符号**， 在BNF中使用

* 终端符号 : 代表一类在语法结构上等效的标记。 终端符号有三种类型：
  * 命名标记:
    这些由 %token 标识符来定义。 按照惯例，它们都是大写。lexer 返回命名标记。
  * 字符标记 : 字符常量的写法与 C 相同。例如, -- 就是一个字符标记。
  * 字符串标记 : 写法与 C 的字符串常量相同。例如，"
    &lt;
    &lt;
    " 就是一个字符串标记。
* 非终端符号 : 是一组非终端符号和终端符号组成的符号。 按照惯例，它们都是小写。

C语言的例子：

[http://www.quut.com/c/ANSI-C-grammar-y-1998.html\#external-declaration](http://www.quut.com/c/ANSI-C-grammar-y-1998.html#external-declaration)

[http://www.quut.com/c/ANSI-C-grammar-l-1998.html](http://www.quut.com/c/ANSI-C-grammar-l-1998.html)

**一、词法分析器生成工具 Lex**

Lex 为词法分析器或扫描器生成Ｃ程序代码。

* 使用DFA引擎，匹配正则表达式匹配输入的字符串并且把它们转换成对应的标记，此标记称为终结符（terminals）或者 记号（tokens），可以给yacc使用。
* 如果两个范式匹配相同的字符串，就会使用匹配长度最长的范式。如果两者匹配的长度相同，就会选用第一个列出的范式。
* 一个 Lex 程序分为三个段：第一段是 C 和 Lex 的全局声明，第二段包括模式（C 代码），第三段是补充的 C 函数。
* 第一段中：出现在
  **%{**
  和
  **%}**
  之间的部分之间复制到文件lex.yy.c中，它们不当做正则表达式处理。
* 尽管 lex 和 yacc 几乎总是被同时提到，但是它们可以单独使用。有很多非常有趣、完全用 lex 编写的小程序。使用 yacc 而 不使用 lex 的程序比较罕见。下面是可以单独执行的一个lex例子，此例子中：

  ```
         如果是lex程序，则可能有如下说明：
  ```

> 第一段只有C的全局声明；_没有token匹配定义；没有include yacc生成的头文件，此文件可能包含命名标记token的定义；_
>
> 第二段_没有return token 终结符给yacc使用；_
>
> 第三段必须手动添加yywrap定义；_如果和yacc联合使用，则main函数转移到yacc中。main在lex中调用yylex，而在yacc中则调用yyparse，其间接调用yylex。_
>
> 注意：lex程序的默认输入为stdin。

```
%{
int nchar, nword, nline;
%}
%%
\n { nline++; nchar++; }
[^ \t\n]+ { nword++, nchar += yyleng; }
. { nchar++; }
%%
int main(void) {
yylex();
printf("%d\t%d\t%d\n", nchar, nword, nline);
return 0;
}
int yywrap()
{
return 1;
}
```

**        
**

**Lex 变量**

* yyin
   FILE\* 类型。 它指向 lexer 正在解析的当前文件。默认为标准输入。
* yyout
   FILE\* 类型。 它指向记录 lexer 输出的位置。 缺省情况下，yyin 和 yyout 都指向标准输入和输出。
* yytext
   匹配模式的文本存储在这一变量中（char\*）。
* yyleng
  给出匹配模式的长度。
* yylineno 提供当前的行数信息。 （lexer不一定支持。）

**Lex 函数**

* yylex\(\)
  ```
      这一函数开始分析。 它由 Lex 自动生成。
  ```
* yywrap\(\)
   这一函数在文件（或输入）的末尾调用。 如果函数的返回值是1，就停止解析。 因此它可以用来解析多个文件。 代码可以写在第三段，这就能够解析多个文件。 方法是使用 yyin 文件指针（见上表）指向不同的文件，直到所有的文件都被解析。 最后，yywrap\(\) 可以返回 1 来表示解析的结束。
* yyless\(int n\)
   这一函数可以用来送回除了前n 个字符外的所有读出标记。
* yymore\(\)
   这一函数告诉 Lexer 将下一个标记附加到当前标记后。

**        
**

个人使用心得：

> lex文件：

* * 其有正则表达式，比yacc文件的规则好使用，个人感觉。。。
  * 不能有空格，必须顶行写；
  * 使用变量必须用大括号括起来；
  * 不能有注释；
  * ABNF中描述需要进行转换，如：

> > > filtercomp     = and / or  --&gt; filtercomp     {and} \| {or}
> > >
> > > filterlist     = 1\*filter  --&gt; filterlist     {filter}+
> > >
> > > EXCLAMATION    = %x21 ; exclamation mark \("!"\)  --&gt; EXCLAMATION    \x21
> > >
> > > UTF2    = %xC2-DF UTF0  --&gt; UTF2    \[\xC2-\xDF\]{UTF0}

**        
**

**二、yacc\(Yet Another Compiler Compiler\)，是一个经典的生成语法分析器的工具**

* Yacc 的 GNU 版叫做 Bison。
* Yacc 文件有 .y 后缀。
* yacc 的文法由一个使用BNF文法（BackusNaur form）的变量描述。 BNF 文法规则最初由 JohnBackus和 Peter Naur 发明，并且用于描述Algol60语言。 BNF 能够用于表达上下文无关语言。
* yacc的输入是巴科斯范式（BNF）表达的语法规则以及语法规约的处理代码，Yacc输出的是基于表驱动的编译器，包含输入的语法规约的处理代码部分。
* yacc是开发编译器的一个有用的工具,采用LALR\(1\)语法分析方法。
* Yacc为语法分析器或剖析器生成Ｃ程序代码。 Yacc使用特定的语法规则以便解释从Lex 得到的标记并且生成一棵语法树。
* 是Unix/Linux上一个用来生成编译器的编译器（编译器代码生成器）。yacc生成的编译器主要是用C语言写成的语法解析器（Parser），需要与词法解析器Lex一起使用，再把两部份产生出来的C程序一并编译。yacc本来只在Unix系统上才有，但现时已普遍移植往Windows及其他平台。
* Yacc 程序也用双百分号分为三段。 它们是：声明、语法规则和 C 代码。

* 每个 Yacc 声明段声明了终端符号和非终端符号（标记）的名称，还可能描述操作符优先级和针对不同符号的数据类型。

* yacc 文件的第一部分定义了解析器将要处理和生成的对象。在一些情况下，它可以是空的， 但是更常见的是，它应该至少包含一些%token 指令。这些 指令用来定义 lexer 可以返回的记号。当使用 -d 选项来运行 yacc 时，它会生成一个定义常量的头文件。

* %token --- terminate字符

* %type  --- 非terminate字符

* %union  --- 定义yystype
* %left
* %right
* %noassoc

**三、下面是一个yacc和lex结合的例子：**

**lex file**

_要求所有定义必须顶格对齐；_

_如果没有声明yylval，则采用默认值；_

```
%{
        #include "y.tab.h"
        #include <stdio.h>
        #include <string.h>
        extern char* yylval;
%}
char [A-Za-z]
num [0-9]
eq [=]
name {char}+
age {num}+
%%
{name} { yylval = strdup(yytext);
 return NAME; }
{eq} { return EQ; }
{age} { yylval = strdup(yytext);
return AGE;}
%%
int yywrap(){
  return 1;
}
```

**yacc file**

_要求%%，%{，%}必须顶格对齐_

_当yacc发现一个解析错误，默认动作是调用yyerror        
_

_定义三个token，在lex中被return。_

```
%{
       typedef char* string;
        #define YYSTYPE string
%}
        %token NAME EQ AGE
%%
        file : record file
        | record
        ;
        record : NAME EQ AGE {
        printf("%s is %s years old!!!\n", $1, $3); }
%%
        int main()
        {
        yyparse();
        return 0;
        }
        int yyerror(char *msg)
        {
        printf("Error encountered: %s \n", msg);}
```

```
执行cat y.tab.h
```

```
#ifndef YYERRCODE
#
define
 YYERRCODE 256
#
endif
#
define
 NAME 257
#
define
 EQ 258
#
define
 AGE 259
```

**执行步骤：**

yacc -d file.y

lex file.l

cc lex.yy.c y.tab.c -o file.exe

**执行结果：**

```
a
=5
a is 5 years old!!!


afds
 = 32
  afds is 32 years old!!!


a
=32
a is 32 years old!!!


tt
=

fd
Error encountered: syntax error
```

四、修改后的例子

yacc文件

%union会生成为yylval的定义

```
#
define
 NUMBER 257
#
define
 NAME 258
#
define
 EQ 259
#
define
 AGE 260
typedef
union
 {

long
    value;

char
 *p;
} YYSTYPE;

extern
 YYSTYPE yylval;
```

```
%union
 { long value; char *p; }
%token
<
value
>
  NUMBER

%token
<
p
>
 NAME EQ AGE

%%

        file : record file
        | record
        record : NAME EQ
 NUMBER
 {

printf
(
"
%s
 is
%d
years old!!!\n"
, 
$1
, 
$3
); }

%%
int
 main()
        {
        yyparse();

return
0
;
        }

int
 yyerror(char *msg)
        {

printf
(
"Error encountered: 
%s
 \n"
, msg);}
```

**lex文件**

_删除了yylval声明_

_        
_

_        
_

```
%{

#
include
 "y.tab.h"
#
include
<
stdio.h
>
#
include
<
string.h
>

%}

char
 [A-Za-z]
num [
0
-
9
]
eq [=]
name {
char
}+
age {num}+
%%
{name} { yylval.p = strdup(yytext);

return
 NAME; }
{eq} { 
return
 EQ; yylval.p = strdup(yytext);}
{age}   {
                yylval.value = strtol(yytext, 
0
, 
10
);

return
 NUMBER;
        }
%%

int
yywrap
()
{

return
1
;
}
%{

#
include
 "y.tab.h"
#
include
<
stdio.h
>
#
include
<
string.h
>

%}

char
 [A-Za-z]
num [
0
-
9
]
eq [=]
name {
char
}+
age {num}+
%%
{name} { yylval.p = strdup(yytext);

return
 NAME; }
{eq} { 
return
 EQ; yylval.p = strdup(yytext);}
{age}   {
                yylval.value = strtol(yytext, 
0
, 
10
);

return
 NUMBER;
        }
%%

int
yywrap
()
{

return
1
;}
```

在lex和yacc文件中声明、定义函数；

在lex文件中一般需要执行yylval = strdup\(yytext\); 这个yylval需要在yacc中delete。

在yacc文件中一般需要执行字符串串联，如$1和$2需要串起来，采用strdup存储到$$中，否则无法向上反馈。

调试方法，lex文件编译时lex -d file.l；而yacc文件的main中定义yy\_flex\_debug = 1；这样cc编译才能通过！

lex文件过滤掉空格等，否则有时有莫名其妙的空格打印。\[ \t\v\n\f\] {}

为了检测出不应该出现的字符，lex文件最后加：.   {return ERROR;}匹配检查，但同时要过滤掉“\0" {}匹配动作，否则总是失败。

```

```



