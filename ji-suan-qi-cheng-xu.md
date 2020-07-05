

# 词法分析程序

```
%{
#include "ch3-01.tab.h"
extern int yylval;
%}

%%
[0-9]+    { yylval = atoi(yytext); return NUMBER; }
[ \t]    ;        /* ignore white space */
\n    return 0;    /* logical EOF */
.    return yytext[0];
%%
```

# 语法分析程序

```
%token NAME NUMBER
%%
statement:	NAME '=' expression
	|	expression		{ printf("= %d\n", $1); }
	;

expression:	expression '+' NUMBER	{ $$ = $1 + $3; }
	|	expression '-' NUMBER	{ $$ = $1 - $3; }
	|	NUMBER			{ $$ = $1; }
	;
%%
int main()
{
	yyparse();
	return 0;
}

int yyerror(char *s)
{
	printf("%s/n",s);
	return 0;

}
```



