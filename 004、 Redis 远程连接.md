### 004、 Redis 远程连接

### 修改密码

#### 一、初始化Redis的密码

​	总共2个步骤：

​	a.在配置文件中有个参数： requirepass  这个就是配置redis访问密码的参数。

​	比如 requirepass redis

​	b.配置文件中参数生效需要重启重启redis 。

 

#### 二、不重启redis如何配置密码?

​	a. 在配置文件中配置requirepass的密码（当redis重启时密码依然有效）。

> ​        requirepass foobared

​	如  修改成 :

> ​       requirepass  redis 

​	b. 进入redis重定义参数

​	查看当前的密码：

``` shell
[root@localhost redis-3.2.8]# ./src/redis-cli -p 6379
redis 127.0.0.1:6379> 
redis 127.0.0.1:6379> config get requirepass
1) "requirepass"
2) ""
```

​	显示密码是空的，

​	然后设置密码：

``` shell
redis 127.0.0.1:6379> config set requirepass redis
OK
```

​	再次查询密码：

``` shell
redis 127.0.0.1:6379> config get requirepass
(error) ERR operation not permitted
```


​	此时报错了！

​	现在只需要密码认证就可以了。

``` shell
redis 127.0.0.1:6379> auth redis
OK
```

​	再次查询密码：

``` shell
redis 127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "redis"
```


​	密码已经得到修改。

​	当到了可以重启redis的时候 由于配置参数已经修改 所以密码会自动生效。

​	要是配置参数没添加密码 那么redis重启 密码将相当于没有设置。

#### 三.如何登录有密码的redis？

​	a.在登录的时候 密码就输入

``` shell
[root@localhost redis-3.2.8]# ./src/redis-cli -p 6379 -a redis
redis 127.0.0.1:6379> 
redis 127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "redis" 
```

​	b.先登录再验证：

``` shell
[root@localhost redis-3.2.8]#  ./src/redis-cli -p 6379
redis 127.0.0.1:6379> 
redis 127.0.0.1:6379> auth redis
OK
redis 127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "redis"
redis 127.0.0.1:6379>
```

#### 四. master 有密码,slave 如何配置？

​	当master 有密码的时候 配置slave 的时候 相应的密码参数也得相应的配置好。不然slave 是无法进行正常复制的。

​	相应的参数是：

​		masterauth

​	比如:

> ​	    masterauth  mstpassword



### 修改IP

``` shell
# By default Redis listens for connections from all the network interfaces
# available on the server. It is possible to listen to just one or multiple
# interfaces using the "bind" configuration directive, followed by one or
# more IP addresses.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1
bind 127.0.0.1
```

修改为：

``` shel
# By default Redis listens for connections from all the network interfaces
# available on the server. It is possible to listen to just one or multiple
# interfaces using the "bind" configuration directive, followed by one or
# more IP addresses.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1
```

或者绑定上你本地的ip