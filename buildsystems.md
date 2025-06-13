# 不要使用cmake,meson,scons,whatever... 老老实实用GNU/Autotools

## cmake的问题

标题就是我的忠告！是的，最近在维护自用的软件依赖包的时候，发现系统升级后只有
cmake 4.0了，然后一堆的用cmake的依赖包没法config。查cmake的升级文档后，好吧！你
cmake真是人才，是第一天才做软件工程的吗？它居然不兼容了许多cmake 3.x的语法！之前
这些语法就会报warnning，但是没有人理它，现在好了，升级4.0后直接是报错。修改这几
十个使用cmake的依赖软件是不可能的了，重新在系统上安装个cmake 3.x吧。（有个环境变
量假模假样的可以保持cmake 3.x兼容，但是我测试没有成功。）

我用的简单的编译管理系统是用的cmake 的ExternalProject_Add()，本来是贪图它简单便
利，现在看来还是自己写个shell script来编译这些依赖软件包吧。

自己写的library也还是用的cmake做build system，决定花个几天改成Autotools吧。

我开始使用cmake的时候还是KDE切换build system的那次。我本来是想KDE那帮老小子眼光
还不错，应该cmake不会太差劲。事实证明随着时间的推移什么都可能改变，不是当年KDE选
错了方案，而是cmake变了。

## meson, scons之类的问题更可怕

为什么这些东西有着更为可怕的问题？因为python！meson设计之初就说我们不直接用
python，是一门独立的DLS，balabalabala，其实者meson的实现只有python，你要不也提供
一个c实现？

为什么python有问题？对于维护软件超过10年的人来说，python造成的困扰巨大！不理解吗？
那看看为什么有那么多工具venv? uv?等等的东西都在解决一个什么问题？哈！是的，版本
不兼容问题。python2 -> python3造成了巨大困扰，后面众人花了小十年把python2的关键
库移植到了python3，可称之为奇迹！但是python3.x -> python3.y的每一次升级都是小型的py2 to py3。

任何一个直接面对用户或者开发者的软件，高版本不兼容低版本的升级都是在自杀！君不见
perl6吗？现在perl6去哪里了？啊！perl6呢？

## POSIX shell 是你的老朋友

上大学的时候就开始接触这个家伙了，但是觉得太复杂，各种宏定义、预处理什么的。后来
新的东西慢慢浮现，浮躁的我就开始骂这位老朋友了，现在虚度光阴20年后发现最可靠的还
是陪伴我学生时代写软件的这位老朋友。（还有一个老哥们是Emacs！）

为什么autotools可靠，就是因为它只是依赖目标系统上有个posix shell。bash, ksh,
zsh, fish等等都是兼容POSIX shell（C shell除外）。而/bin/sh这个可执行文件至少已经
稳定了50多年了，不要小看这个稳定性，它以前和可预见的将来不太可能出现某个版本的
/bin/sh就没法运行现有的shell脚本了。

再加上M4、sed、perl5等等这些传统的GNU老工具，带来的就是让人感觉可靠。虽然老旧但
是可靠，不用反复折腾。
