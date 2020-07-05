从下列地方可以等到lex 和yacc 工具：



> ● Mortice Kern System \(MKS\)，在 www.mks.com，
>
> ● GNU flex 和 bison，在 www.gnu.org ，
>
> ● Ming，在 www.mingw.org，
>
> ● Cygwin，在 www.cygwin.org



“lex”和“yacc”这两个名字所代表的也包括这些工具的 GNU 版本 flex 和 bison

  


**终端和非终端符号**， 在BNF中使用  


* 终端符号 : 代表一类在语法结构上等效的标记。 终端符号有三种类型：
  * 命名标记:
    这些由 %token 标识符来定义。 按照惯例，它们都是大写。lexer 返回命名标记。
  * 字符标记 : 字符常量的写法与 C 相同。例如, -- 就是一个字符标记。
  * 字符串标记 : 写法与 C 的字符串常量相同。例如，"
    &lt;
    &lt;
    " 就是一个字符串标记。
* 非终端符号 : 是一组非终端符号和终端符号组成的符号。 按照惯例，它们都是小写。

  


C语言的例子：

http://www.quut.com/c/ANSI-C-grammar-

