## service redis does not support chkconfig的解决办法

问题解决办法如下：

必须把下面两行注释放在/etc/init.d/redis文件靠前的注释中：

``` shell
# chkconfig: 2345 90 10
# description: Redis is a persistent key-value database
```

 

上面的注释的意思是，redis服务必须在运行级2，3，4，5下被启动或关闭，启动的优先级是90，关闭的优先级是10。

 

# 附录：

## linux 运行级别

运行级别就是操作系统当前正在运行的功能级别。这个级别从0到6 ，具有不同的功能。这些级别在/etc/inittab文件里指定。这个文件是init程序寻找的主要文件，最先运行的服务是那些放在/etc/rc.d 目录下的文件。

不同的运行级定义如下：(可以参考Linux里面的/etc/inittab)

 

\# 缺省的运行级，RHS用到的级别如下：

0：关机

1：单用户模式

2：无网络支持的多用户模式

3：有网络支持的多用户模式

4：保留，未使用

5：有网络支持有X-Window支持的多用户模式

6：重新引导系统，即重启

 

对各个运行级的详细解释：

 

0 为停机，机器关闭。

1 为单用户模式，就像Win9x下的安全模式类似。

2  为多用户模式，但是没有NFS支持。 

3  为完整的多用户模式，是标准的运行级。

4 一般不用，在一些特殊情况下可以用它来做一些事情。例如在笔记本 电脑的电池用尽时，可以切换到这个模式来做一些设置。

5  就是X11，进到X Window系统了。

6  为重启，运行init 6机器就会重启。

 

## chkconfig用法

chkconfig命令可以用来检查、设置系统的各种服务

使用语法：

chkconfig [--add][--del][--list][系统服务] 或 chkconfig [--level <等级代号>][系统服务][on/off/reset]

 

参数用法：

–add 　增加所指定的系统服务，让chkconfig指令得以管理它，并同时在系统启动的叙述文件内增加相关数据。

–del 　删除所指定的系统服务，不再由chkconfig指令管理，并同时在系统启动的叙述文件内删除相关数据。

–level<等级代号> 　指定读系统服务要在哪一个执行等级中开启或关毕。

 

使用范例：

chkconfig –list                    列出所有的系统服务

chkconfig –add redis               增加redis服务

chkconfig –del redis                删除redis 服务

chkconfig –level redis 2345 on     把redis在运行级别为2、3、4、5的情况下都是on（开启）的状态。



#### 原文链接：

## [service redis does not support chkconfig的解决办法](http://www.cnblogs.com/goodspeed/archive/2012/10/18/2729615.html)