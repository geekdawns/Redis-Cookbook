### 002、 Redis 安装

#### Installation

Download, extract and compile Redis with:

```
$ wget http://download.redis.io/releases/redis-3.2.6.tar.gz
$ tar xzf redis-3.2.6.tar.gz
$ cd redis-3.2.6
$ make
```

The binaries that are now compiled are available in the `src` directory. Run Redis with:

```
$ src/redis-server
```

You can interact with Redis using the built-in client:

```
$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

Are you new to Redis? Try our [online, interactive tutorial](http://try.redis.io/).

``` she
12536:C 15 Dec 15:22:15.443 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
12536:M 15 Dec 15:22:15.444 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 3.0.3 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 12536
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

12536:M 15 Dec 15:22:15.446 # Server started, Redis version 3.0.3
12536:M 15 Dec 15:22:15.446 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
12536:M 15 Dec 15:22:15.446 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
12536:M 15 Dec 15:22:15.446 * The server is now ready to accept connections on port 6379
^C12536:signal-handler (1481786562) Received SIGINT scheduling shutdown...
12536:M 15 Dec 15:22:42.636 # User requested shutdown...
12536:M 15 Dec 15:22:42.636 * Saving the final RDB snapshot before exiting.
12536:M 15 Dec 15:22:42.639 * DB saved on disk
12536:M 15 Dec 15:22:42.639 # Redis is now ready to exit, bye bye...

```

