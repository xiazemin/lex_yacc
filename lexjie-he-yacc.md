lex：就是用来提取符合一定规则的内容

yacc：分析lex提取过来的内容，然后做进一步操作。

三.Lex\(Lexical Analyzar\) 一些的内部变量和函数

内部预定义变量：

yytext   char \*  当前匹配的字符串  
yyleng   int     当前匹配的字符串长度  
yyin     FILE \*  lex当前的解析文件，默认为标准输出  
yyout    FILE \*  lex解析后的输出文件，默认为标准输入  
yylineno int     当前的行数信息

内部预定义宏：

ECHO     \#define ECHO fwrite\(yytext, yyleng, 1, yyout\)  也是未匹配字符的  
         默认动作  


内部预定义的函数：

int yylex\(void\)    调用Lex进行词法分析  
int yywrap\(void\)   在文件\(或输入\)的末尾调用。如果函数的返回值是1，就停止解  
                   析。 因此它可以用来解析多个文件。代码可以写在第三段，这  
                   样可以解析多个文件。 方法是使用 yyin 文件指针指向不同的  
                   文件，直到所有的文件都被解析。最后，yywrap\(\) 可以返回1  
                   来表示解析的结束。  
  
  
lex和flex都是解析Lex文件的工具，用法相近，flex意为fast lexical analyzer generator。  
可以看成lex的升级版本。

  
相关更多内容就需要参考flex的man手册了，十分详尽。



四.关于Lex的一些综述

Lex其实就是词法分析器，通过配置文件\*.l，依据正则表达式逐字符去顺序解析文件，并动态更新内存的数据解析状态。不过Lex只有状态和状态转换能力。因为它没有堆栈，它不适合用于剖析外壳结构。而yacc增加了一个堆栈，并且能够轻易处理像括号这样的结构。Lex善长于模式匹配，如果有更多的运算要求就需要yacc了。

