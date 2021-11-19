# 通过源代码 port 方式安装软件

## FreeBSD ports 基本用法 <a href="freebsdports-ji-ben-yong-fa" id="freebsdports-ji-ben-yong-fa"></a>

首先获取portsnap\
\#portsnap fetch extract

***

使用whereis 查询软件地址\
如#whereis python 回应：\
\#whereis python\
输出\
python: /usr/ports/lang/python

***

如何安装python3：\
\#cd /usr/ports/lang/python\
\#make BATCH=yes clean\
其中BATCH=yes 的意思是使用默认配置

***

如何使用多线程下载：\
\#pkg install axel #下载多线程下载工具#\
新建或者编辑/etc/make.conf文件，写入以下两行：\
FETCH\_CMD=axel\
FETCH\_BEFORE\_ARGS= -n 10 -a\
FETCH\_AFTER\_ARGS=\
DISABLE\_SIZE=yes

***

进阶：如果不选择BATCH=yes 的方法手动配置依赖：\
看看python 的ports 在哪：\
\#whereis python\
python: /usr/ports/lang/pytho\
安装python3：\
\#cd /usr/ports/lang/python\
如何设置全部所需的依赖：\
\#make config-recursive\
如何一次性下载所有需要的软件包：\
\#make BATCH=yes fetch-recursive

三.升级 ports collection

1 portsnap fetch extract\
四.FreeBSD 包升级管理工具\
首先更新Ports树\
portsnap fetch update\
然后列出过时Ports组件\
pkg\_version -l ‘<’\
下边分别列出2种FreeBSD手册中提及的升级工具:\
一、portupgrade\
cd /usr/ports/ports-mgmt/portupgrade &\&make install clean\
portupgrade -ai #自动升级所有软件\
portupgrade -R screen #升级单个软件

二、portmaster （推荐）

cd /usr/ports/ports-mgmt/portmaster && make install clean\
portmaster -ai #自动升级所有软件\
portmaster screen#升级单个软件\
portmaster -a -m “BATCH=yes” 或者-D -G –no-confirm 都可以免除确认

## FreeBSD ports 多线程编译

linux如gentoo上一般是直接 -jx 或者-jx+1 x为核心数。\
FreeBSD ports 多线程编译\
FORCE\_MAKE\_JOBS=yes\
MAKE\_JOBS\_NUMBER=4\
写入/etc/make.conf\
没有就新建。4是处理器核心数，不知道就别改。\
\#其他见 /usr/ports/Mk/bsd.port.mk