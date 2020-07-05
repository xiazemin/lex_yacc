变量和有类型的标记

下一步扩展计算器来处理具有单个字母名字的变量，因为只有26个字母 （目前只关心小写字母），所以我们能在26个条目的数组（称它为vbltable）中存储变量。

为了使得计算器更加有用，也可以扩展它来处理多个表达式（每行一个）和使用浮点值。

具有变量和实值的计算器词法

```
%{
    #include "num.tab.h"
    #include <math.h>
    extern double vbltable[26];
%}
%option noyywrap
%%
([0-9]+|([0-9]*\.[0-9]+)([eE][-+]?[0-9]+)?)    {
        yylval.dval = atof(yytext);
        return NUMBER;
    }
[ \t]    ;        /*忽略空白*/
[a-z]    {    yylval.vblno = yytext[0] - 'a';
            return NAME;
        }
"$"        {    return 0; /*输入结束*/ }
\n        |
.        return yytext[0];
%%
```

## 具有变量和实值的计算器语法

```
%{
    double vbltable[26];
%}

%union {
        double dval;
        int vblno;
    }
%token    <vblno> NAME
%token    <dval>    NUMBER
%left     '-' '+'
%left    '*'    '/'
%nonassoc     UMINUS

%type <dval> expression

%%
statement_list: statement '\n'
    |    statement_list statement '\n'
statement:    NAME '=' expression        {vbltable[$1] = $3; }
    |    expression        { printf("= %g\n", $1); }
    ;

expression:    expression '+' expression    { $$ = $1 + $3; }
    |        expression '-' expression    { $$ = $1 - $3; }
    |        expression '*' expression   { $$ = $1 * $3; }
    |        expression '/' expression
                        {
                            if($3 == 0.0)
                                yyerror("divide by zero");
                            else
                                $$ = $1 / $3;
                        }
    |    '-' expression %prec UMINUS {$$ = -$2;}
    |    '(' expression ')'    {$$ = $2; }
    |    NUMBER            { $$ = $1; }
    |    NAME            { $$ = vbltable[$1]; }
    ;
%%

int main()
{
    yyparse();
    return 0;
}

int yyerror(char *s)
{
    printf("%s\n",s);
    return 0;

}
```

```
bison -d num.y
flex num.l
gcc num.tab.c lex.yy.c  -o n
```

```
$./n
a=2/3
b=a+1
2/3
= 0.666667
b/4
= 0.416667
b/a
= 2.5
```

flex.exe和bison.exe是UnxUtils包中的文件，已经将许多Unix/Linux平台的程序都移植到了Windows平台，可以直接到UnxUtils网站下载，下载解压缩之后在系统的PATH环境变量中增加UnxUtils所有的exe文件所在的目录，使 得DOS命令行可以直接搜索到flex.exe和bison.exe，除此之外还需要从网络上下载 bison需要的bison.simple和bison.hairy两个文件，并且还要分别设置环境变量 BISON\_HAIRY指向bison.hairy，BISON\_SIMPLE指向bison.simple。 然后，打开cmd检查是否安装成功 flex --version ，bison --version

[https://blog.csdn.net/fly\_yr/article/details/43015929?locationNum=10&fps=1](https://blog.csdn.net/fly_yr/article/details/43015929?locationNum=10&fps=1)

