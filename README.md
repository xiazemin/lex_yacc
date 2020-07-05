# Introduction

解析 C 语言源程序，

编写 脚本引擎等等，解决这种文本解析的方法有很多，一种方法就是自己手动

用 C 或者 C++直接编写解析程序，这对于简单格式的文本信息来说，不会是什么

问题，但是 对于稍微复杂一点的文本信息的解析来说，手工编写解析器将会是

一件漫长痛苦 而容易出错的事情。本系列文档就是专门用来由浅入深的介绍两

个有名的 Unix 工 具 Lex 和 Yacc，并会一步一步的详细解释如何用这两个工具

来实现我们想要的任何 功能的解析程序，为了方便理解和应用，我会在该系列

的文章中尽可能的采用具 体可行的实例来加以阐释，而且这种实例都是尽可能

的和具体的系统平台无关的 ，因此我采用命令行程序作为我们的解析程序的最

终结果。

