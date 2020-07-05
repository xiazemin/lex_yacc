Lex\(Lexical Analyzar\) 一些的内部变量和函数

内部预定义变量：

yytext   char \* 当前匹配的字符串  
yyleng   int     当前匹配的字符串长度  
yyin     FILE \* lex当前的解析文件，默认为标准输出  
yyout    FILE \* lex解析后的输出文件，默认为标准输入  
yylineno int     当前的行数信息

内部预定义宏：

ECHO     \#define ECHO fwrite\(yytext, yyleng, 1, yyout\) 也是未匹配字符的  
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

