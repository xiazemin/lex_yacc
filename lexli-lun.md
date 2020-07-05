Lex\(Lexical Analyzar\) 一些的内部变量和函数

内部预定义变量：

yytext   char \* 当前匹配的字符串  
yyleng   int     当前匹配的字符串长度  
yyin     FILE \* lex当前的解析文件，默认为标准输出  
yyout    FILE \* lex解析后的输出文件，默认为标准输入  
yylineno int     当前的行数信息

内部预定义宏：

ECHO     \#define ECHO fwrite\(yytext, yyleng, 1, yyout\) 也是未匹配字符的  
         默认动作

内部预定义的函数：

int yylex\(void\)    调用Lex进行词法分析  
int yywrap\(void\)   在文件\(或输入\)的末尾调用。如果函数的返回值是1，就停止解  
                   析。 因此它可以用来解析多个文件。代码可以写在第三段，这  
                   样可以解析多个文件。 方法是使用 yyin 文件指针指向不同的  
                   文件，直到所有的文件都被解析。最后，yywrap\(\) 可以返回1  
                   来表示解析的结束。

lex和flex都是解析Lex文件的工具，用法相近，flex意为fast lexical analyzer generator。  
可以看成lex的升级版本。



4.Lex理论

Lex使用正则表达式从输入代码中扫描和匹配字符串。每一个字符串会对应一个动作。  
通常动作返回一个标记给后面的剖析器使用，代表被匹配的字符串。每一个正则表达  
式代表一个有限状态自动机\(FSA\)。我们可以用状态和状态间的转换来代表一个\(FSA\)。  
其中包括一个开始状态以及一个或多个结束状态或接受状态。

5.关于Lex的一些综述

Lex其实就是词法分析器，通过配置文件\*.l，依据正则表达式逐字符去顺序解析文件，  
并动态更新内存的数据解析状态。不过Lex只有状态和状态转换能力。因为它没有堆栈，  
它不适合用于剖析外壳结构。而yacc增加了一个堆栈，并且能够轻易处理像括号这样的  
结构。Lex善长于模式匹配，如果有更多的运算要求就需要yacc了。



6、yacc的BNF文件

    个人认为lex理论比较容易理解的，yacc要复杂一些。   
    我们先从yacc的文法说起。yacc文法采用BNF\(Backus-Naur Form\)的变量规则描  
述。BNF文法最初由John Backus和Peter Naur发明，并且用于描述Algol60语言。BNF  
能够用于表达上下文无关的语言。现代程序语言大多数结构能够用BNF来描述。

[http://www.voidcn.com/article/p-gcsrqzqq-cx.html](http://www.voidcn.com/article/p-gcsrqzqq-cx.html)

