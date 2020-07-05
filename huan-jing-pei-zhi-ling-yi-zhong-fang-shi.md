必备工具

flex-2.5.4a-1.exe   和  bison-2.4.1-setup.exe   以及 cygwin2.738 的安装文件,下载地址 [http://download.csdn.net/detail/fly\_yr/8385245](http://download.csdn.net/detail/fly_yr/8385245)

flex与bison安装

运行flex-2.5.4a-1.exe  和  bison-2.4.1-setup.exe 文件安装至D:\Software Files\GnuWin32下，然后按配置环境变量：

将路径 D:\Software Files\GnuWin32\bin 复制于Path中。

Cygin安装配置

运行cyg\_win\_setup.exe文件安装至D:\cygwin下，然后配置环境变量：

将路径D:\cygwin\bin复制于Path中；

注意：在D:\cygwin\bin文件夹下，有g++.exe 大小为1KB   与g++ 3.exe 大小为95KB ， 我们需要把95KB的 g++ 3.exe命名为 g++.exe ，1 KB的g++可删除，或者命名为g++3.exe；

同理，有gcc.exe 大小为1KB   与gcc 3.exe 大小为95KB ， 我们需要把95KB的 gcc 3.exe命名为 gcc.exe ，1 KB的gcc可删除，或者命名为gcc 3.exe 。

必要文件复制

我们发现D:\cygwin\bin下面并没有flex.exe 与bison.exe，因此，

（1）将安装好的D:\Software Files\GnuWin32\bin下的flex.exe 与bison.exe复制到D:\cygwin\bin下面；

（2）再将D:\Software Files\GnuWin32的share文件夹复制到D:\cygwin下面；

（3）将D:\Software Files\GnuWin32\lib下的libfl.a 和 liby.a 复制到D:\cygwin\lib下面；

检测配置是否成功

打开D:\cygwin下的Cygwin.bat 或者 系统的cmd，按照以下方式检验：

[https://blog.csdn.net/fly\_yr/article/details/43015189](https://blog.csdn.net/fly_yr/article/details/43015189)

