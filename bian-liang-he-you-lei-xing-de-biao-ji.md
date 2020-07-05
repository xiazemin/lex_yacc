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



