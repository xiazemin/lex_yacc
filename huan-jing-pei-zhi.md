## 1.1 必备工具（备注：所需工具均在我的资源文件中可找到）

Windows平台下面Lex 和Yacc开发环境所需要安装的程序：  


（1）Lex（flex.exe）

（2）Yacc（bison.exe）

（3）C/C++编译器

## 1.2 flex和bison安装

flex.exe和bison.exe是UnxUtils包中的文件，已经将许多Unix/Linux平台的程序都移植到了Windows平台，可以直接到UnxUtils网站下载，下载解压缩之后在系统的PATH环境变量中增加UnxUtils所有的exe文件所在的目录，使 得DOS命令行可以直接搜索到flex.exe和bison.exe，除此之外还需要从网络上下载 bison需要的bison.simple和bison.hairy两个文件，并且还要分别设置环境变量 BISON\_HAIRY指向bison.hairy，BISON\_SIMPLE指向bison.simple。

  


然后，打开cmd检查是否安装成功，如下图所示：



![](http://img.blog.csdn.net/20150109103203506?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmx5X3ly/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


## 1.3 C/C++编译器

我们使用的flex和bison都是GNU的工具，所以为了方便，采用的c/c++编译器也是GNU的编译器GCC，需要WINDOWS版的MinGW编译器，在可以到MinGW的主页下载安装。

安装完毕后，将MinGW下的bin目录添加到系统环境变量的Path中。

  


配置完毕。

[https://www.cnblogs.com/shine-yr/p/5214976.html](https://www.cnblogs.com/shine-yr/p/5214976.html)

