### 008、 Redis 队列示例

入队：

``` php
<?php
$redis = new Redis();
$redis->connect('127.0.0.1',6379);
while(True){
  try{
    $value = 'value_'.date('Y-m-d H:i:s');
    $redis->LPUSH('key1',$value);
    sleep(rand()%3);
    echo $value."\n";
  }catch(Exception $e){
    echo $e->getMessage()."\n";
  }
}
?> 
```

出队：

``` php
<?php
$redis = new Redis();
$redis->pconnect('127.0.0.1',6379);
while(True){
  try{
    echo $redis->LPOP('key1')."\n";
  }catch(Exception $e){
  echo $e->getMessage()."\n";
  }
  sleep(rand()%3);
}
?>
```

**如何使用Redis 做队列操作** 
Reids是一个比较高级的开源key-value存储系统，采用ANSI C实现。其与memcached类似，但是支持持久化数据存储，同时value支持多种类型：字符串 （同memcached中的value），列表 ，集合 (Set)，有序集合 (OrderSet)和Hash 。所有的值类型均支持原子操作，如列表中追加弹出元素，集合中插入移除元素等。Rdids的数据大部分位于内存中，其读写效率非常高，其提供AOF（追加 式操作记录文件）和DUMP（定期数据备份）两种持久化方式。Redis支持自定义的VM（虚拟内存）机制，当数据容量超过内存时，可以将部分Value 存储到文件中。同时Redis支持Master-Slave机制，可以进行数据复制。 
可以把Redis的list结构当队列来用. 
从上面Redis的场景和作用来说，对于我们现在的开发活动，究竟能把Redis引入在那些场景，而不是把这么好的东东演变成“为了使用Redis，而Redis”的惨烈局面呢？当然，具体问题具体分析，这个真的很重要哈。 
缓存？分布式缓存？ 
队列？分布式队列？ 
某些系统应用（例如，电信、银行和大型互联网应用等）都会使用到，当然，现在大行其道的memcache就是很好的证明；但从某一方面来说，memcache是否能把两张囊括其中，而且能做到更好（没有实际的应用过，所以只是抛出）。但从Redis身上，我就能感觉到，Redis，就能把队列和缓存两张都囊括其中，而且都不会产生并发环境下的困扰，因为Redis中的操作都是原子操作来着。 
至于评论两者的孰好孰坏就免了，存在就是理由，选择适合的就是最好的。 
下面开始玩玩Redis中的队列（分布式）设计YY吧，请大虾们多多指点。 
状况场景： 
现在的项目，都是部署在多个服务器，或者多个IP上，而且前台经由F5分发，所以用户的请求究竟落在那一台的服务器上，是无法确定的。对于项目中，有一秒杀设计，刚开始没有考虑到这种部署，同时也是使用最容易处理的方式，直接给数据库表锁行记录（Oracle上的）。可以说，对于不同的应用部署，而只有一台数据库服务器来说，很“轻松”的就解决了这个并发的问题。所以现在考虑一下，是不是挪到应用上，避免数据库服务器也掺杂到业务上。 
比如，现在有2台应用服务器，1台数据库服务器。想法是，把Redis部署在数据库服务器上，两台服务器在操作并发缓存或者队列时，先从Redis服务器上，取得在两台应用服务器的代理对象，再做入列出列的操作。 
看代码实现（PHP） 
入队列操作文件 list_push.php 

``` php
<?php 
$redis = getRedisInstance();//从Redis服务器拿到redis实例 
$redis->connect('Redis服务器IP', 6379); 
while (true) { 
$redis->lPush('list1', 'A_'.date('Y-m-d H:i:s')); 
sleep(rand()%3); 
} 
?> 
```

执行# php list_push.php & 
出队列操作 list_pop.php文件 

``` php
<?php 
$redis = getRedisInstance();//从Redis服务器拿到redis实例 
$redis->pconnect('Redis服务器IP', 6379); 
while(true) { 
try { 
var_export( $redis->blPop('list1', 10) ); 
} catch(Exception $e) { 
//echo $e; 
} 
} 
```

实现方法(Python) 
1.入队列（write.py） 

``` php
#!/usr/bin/env python 
import time 
from redis import Redis 
redis = Redis(host='127.0.0.1', port=6379) 
while True: 
now = time.strftime("%Y/%m/%d %H:%M:%S") 
redis.lpush('test_queue', now) 
time.sleep(1) 
```



2.出队列（read.py） 

``` php
#!/usr/bin/env python 
import sys 
from redis import Redis 
redis = Redis(host='127.0.0.1', port=6379) 
while True: 
res = redis.rpop('test_queue') 
if res == None: 
pass 
else: 
print str(res)  
```

在操作时，注意，要操作的是同一个list对象。 
呵呵，现在的主要思路就差不多就是如此，不过真实场景中，会有出入。