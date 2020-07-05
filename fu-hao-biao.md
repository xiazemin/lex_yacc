符号表

列举单词表的方式虽然简单但是不全面，如果在词法分析程序运行时可以构建一个单词表，那么就可以在添加新的单词时不用修改词法分析程序。

下面示例便利用符号表实现，即在词法分析程序运行时从输入文件中读取声明的单词时允许动态的声明单词。声明以词性的名字开始，后面跟着要声明的单词。

添加符号表可以完全的改变词法分析程序，不必在词法分析程为每个要匹配的单词放置独立的模式，只要有一个匹配任意单词的模式，再查阅符号表就能决定所找到的词性。

## lex程序

```
%{
  /*
   *带符号表的词法分析程序
   */

   enum{
        LOOKUP = 0, /*默认——查找而不是定义*/
        VERB,
        ADJ,
        ADV,
        NOUN,
        PREP,
        PRON,
        CONJ
    };

    int state;
    int add_word(int type, char *word);
    int lookup_word(char *word);
%}

%%
\n        {state = LOOKUP;} /*行结束，返回到默认状态*/

        /*无论何时，行都以保留字的词性名字开始*/
        /*定义该类型的单词*/
^verb     {state = VERB;}
^adj    {state = ADJ;}
^adv    {state = ADV;}
^noun    {state = NOUN;}
^prep    {state = PREP;}
^pron    {state = PRON;}
^conj    {state = CONJ;}

[a-zA-Z]+    {
                /*一个标准的单词、定义或者查找它*/
                if(state != LOOKUP){
                    /*定义当前单词*/
                    add_word(state,yytext);
                }else{
                    switch(lookup_word(yytext)){
                        case VERB: printf("%s: verb\n",yytext); break;
                        case ADJ: printf("%s: adj\n",yytext); break;
                        case ADV: printf("%s: adv\n",yytext); break;
                        case NOUN: printf("%s: noun\n",yytext); break;
                        case PREP: printf("%s: prep\n",yytext); break;
                        case PRON: printf("%s: pron\n",yytext); break;
                        case CONJ: printf("%s: conj\n",yytext); break;
                        default:
                                    printf("%s: don't recognize\n",yytext);
                                    break;
                    }//switch
                }//else
            }
            /*忽略其他的东西*/
%%

int main()
{
    yylex();
}

/*定义一个连接的单词和类型列表*/
struct word{
    char *word_name;
    int word_type;
    struct word *next;
};

struct word *word_list; //first element in word list

extern void *malloc();

int add_word(int type, char *word)
{
    struct word *wp;
    if(lookup_word(word) != LOOKUP)
    {
        printf("!!! warning : word %s already defined \n ",word);
        return 0;
    }//if

    /*单词不在那里，分配一个新的条目并将它连接到列表上*/
    wp = (struct word*) malloc (sizeof(struct word));

    wp->next = word_list;

    /*还必须复制单词本身*/
    wp->word_name = (char *)malloc(strlen(word)+1);
    strcpy(wp->word_name,word);
    wp->word_type = type;
    word_list = wp;
    return 1; /*它被处理过*/
}

int lookup_word(char *word)
{
    struct word *wp = word_list;

    /*向下搜索列表以寻找单词*/
    for(; wp ; wp = wp->next)
    {
        if(strcmp(wp->word_name , word) == 0)
            return wp->word_type;
    }//for
    return LOOKUP;
}

int yywrap()
{
    return 1;
}
```

$lex table.l 

$gcc -o t lex.yy.c 

程序代码说明：

在上述lex词法分析程序中，add\_word\(\)表示在符号表中放置一个新的单词；

lookup\_word\(\)表示查询已经输入的单词。

在程序代码中，声明一个变量state，用来记录是在查找单词（状态LOOKUP）还是在声明它们（在这种情况下，state能记住我们正在声明的单词种类）。

无论何时，只要我们看到以词性名字开始的行，就可以将状态设置为声明单词的种类；每次看到\n时就都切换回正常的查找状态。

