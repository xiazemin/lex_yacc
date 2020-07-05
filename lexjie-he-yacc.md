lex：就是用来提取符合一定规则的内容

yacc：分析lex提取过来的内容，然后做进一步操作。

三.Lex\(Lexical Analyzar\) 一些的内部变量和函数

内部预定义变量：

yytext   char \*  当前匹配的字符串  
yyleng   int     当前匹配的字符串长度  
yyin     FILE \*  lex当前的解析文件，默认为标准输出  
yyout    FILE \*  lex解析后的输出文件，默认为标准输入  
yylineno int     当前的行数信息

内部预定义宏：

ECHO     \#define ECHO fwrite\(yytext, yyleng, 1, yyout\)  也是未匹配字符的  
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

相关更多内容就需要参考flex的man手册了，十分详尽。

四.关于Lex的一些综述

Lex其实就是词法分析器，通过配置文件\*.l，依据正则表达式逐字符去顺序解析文件，并动态更新内存的数据解析状态。不过Lex只有状态和状态转换能力。因为它没有堆栈，它不适合用于剖析外壳结构。而yacc增加了一个堆栈，并且能够轻易处理像括号这样的结构。Lex善长于模式匹配，如果有更多的运算要求就需要yacc了。

[http://www.voidcn.com/article/p-skdphhhz-bbp.html](http://www.voidcn.com/article/p-skdphhhz-bbp.html)



[http://www.voidcn.com/article/p-pejeuief-zg.html](http://www.voidcn.com/article/p-pejeuief-zg.html)

Lex和Yacc学习过程中遇到的几个问题

1.在Lex中表述空格，空格的表述得使用\[ \] 或者 " " ，不可以直接写空格，否则是匹配不了的。

2.在同时使用Lex和Yacc的时候，如果我们不想编写main函数或者相关的配套函数，

   例如Lex的yywrap，Yacc的yyerror等，我们可以直接使用Lex或者Yacc提供的链接库

   -ll 和-ly ,但是特别要注意的是，此处特别要注意（这个问题查了我三四天的时间），一定要

   将-ly 写在-ll的前面，因为-ly和-ll均实现并导出了main，而在使用的过程中如果-ll写在了前面

   就会导致Yacc的执行根本就不会执行

[http://www.voidcn.com/article/p-fgflomos-bqy.html](http://www.voidcn.com/article/p-fgflomos-bqy.html)

[http://www.voidcn.com/article/p-ogikbtcy-tg.html](http://www.voidcn.com/article/p-ogikbtcy-tg.html)

