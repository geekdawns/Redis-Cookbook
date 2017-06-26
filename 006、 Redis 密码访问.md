### 006、 Redis 密码访问

**一、首先设置Redis密码，以提供远程登陆**

打开redis.conf配置文件，找到requirepass，然后修改如下:

```
requirepass yourpassword
```

yourpassword就是redis验证密码，设置密码以后发现可以登陆，但是无法执行命令了。

命令如下:

```
redis-cli -h 127.0.0.1 -p 6379//启动redis客户端，并连接服务器
```

```
keys * //输出服务器中的所有key
```

报错如下

```
(error) ERR operation not permitted
```

这时候你可以用授权命令进行授权，就不报错了

命令如下:

```
auth youpassword
```

**二、PHP访问Redis**

```
$redis = new Redis();
$conn = $redis->connect('localhost', 6379);
$auth = $redis->auth('20160601'); //设置密码
var_dump($auth);
$redis->set('access_token', "123213213213213213");
$redis->set('expired_time', 1464344863);

var_dump($redis->get("access_token"));
var_dump($redis->get("expired_time"));
```