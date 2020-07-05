用lex识别单词

构建一个识别不同类型英语单词的简单程序。先识别词性（名词，动词等），然后再扩展到处理符合简单英语语法的多个单词的句子。

先列出要识别的一组动词：

is    am   are   were   was   be  being  been   do  does  did  will   would  should  can  could  has  have  had  go

识别这些动词的lex程序：

```
%{
/*
 * 例1-1 单词识别程序 ch1-02.l
 * 识别单词是否是动词
 */
%}
%%
[\t ]+          /*意味着空格的正闭包，忽略空白*/; 
is |
am |
are |
were |
was |
be |
being |
been |
do |
does |
did |
will |
would |
should |
can |
could |
has |
have |
had |
go        {printf("%s: is a verb\n",yytext);}
[a-zA-Z]+        {printf("%s: is not a verb\n",yytext);}

.|\n        {ECHO;/*通常的默认状态：输出匹配模式，复制标点或其它字符*/}
%%

int main()
{
    yyin = fopen("example.txt","r");
        yylex();
    fclose(yyin);
}
int yywrap()
{
    return 1;
}
```

## 需要的例子文件example.txt （放在相同文件夹）：

```
did I have fun?
```

lex word.l

gcc -o w lex.yy.c

./w

did: is a verb

I: is not a verb

have: is a verb

fun: is not a verb

[https://blog.csdn.net/fly\_yr/article/details/42643217?utm\_source=blogxgwz3](https://blog.csdn.net/fly_yr/article/details/42643217?utm_source=blogxgwz3)

