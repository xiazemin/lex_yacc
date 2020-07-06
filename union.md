通常解决此问题的方法是定义一个容纳 yacc 将要处理 的对象的数据类型。这个数据类型是一个 C union 对象，在 yacc 文件的第一部分使用 %union 声明来定义。定义了记号以后，可以为 它们指定一个类型。例如，对于一个玩具级的编程语言来说，您可以像下面这样做：



清单 4. 一个最简化的 %union 声明

%union {

    long    value;

}

%token &lt;value&gt;    NUMBER

%type &lt;value&gt; expression

这就表明，当解析器得到 lexer 返回的 NUMBER 记号时，它可以 认为全局变量 yylval 的名为 value 的 成员已经被赋与了有意义的值。当然，您的 lexer 必须要以某种方式来处理它：



清单 5. 使一个 lexer 使用 yylval

\[0-9\]+  {

        yylval.value = strtol\(yytext, 0, 10\);

        return NUMBER;

    }

yacc 允许您通过符号名引用表达式的组成部分。当解析非终结符时，进入解析器的组成部分被 命名为 $1 、 $2 ，依次类推；它将向 高层解析器返回的值名为 $$ 

