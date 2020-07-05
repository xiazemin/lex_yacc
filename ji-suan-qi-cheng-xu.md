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
statement:    NAME '=' expression
    |    expression        { printf("= %d\n", $1); }
    ;

expression:    expression '+' NUMBER    { $$ = $1 + $3; }
    |    expression '-' NUMBER    { $$ = $1 - $3; }
    |    NUMBER            { $$ = $1; }
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

$lex num.l

$bison -d num.y

$ls

lex.yy.c        num.tab.c       num.y

num.l           num.tab.h

flex num.l

bison -d  num.y

gcc lex.yy.c -o n

```
$ gcc lex.yy.c -o n
Undefined symbols for architecture x86_64:
  "_main", referenced from:
     implicit entry/start for main executable
  "_yylval", referenced from:
      _yylex in lex-501ddf.o
  "_yywrap", referenced from:
      _yylex in lex-501ddf.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

$ gcc lex.yy.c -o n -v

```
End of search list.
 "/Library/Developer/CommandLineTools/usr/bin/ld" -demangle -dynamic -arch x86_64 -macosx_version_min 10.11.0 -o n /var/folders/r9/35q9g3d56_d9g0v59w9x2l9w0000gn/T/lex-ae1cef.o -lSystem /Library/Developer/CommandLineTools/usr/bin/../lib/clang/8.0.0/lib/darwin/libclang_rt.osx.a
Undefined symbols for architecture x86_64:
  "_main", referenced from:
     implicit entry/start for main executable
```

解决办法

$ gcc num.tab.c  -o n -v

```

```



