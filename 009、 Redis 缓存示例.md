### 009、 Redis 缓存示例

``` php
<?php
/**
 * Redis缓存操作
 * @author hxm
 * @version 1.0
 * @since 2015.05.04
 */
class RCache extends Object implements CacheFace 
{
  private $redis = null; //redis对象
   
  private $sId  = 1;  //servier服务ID
   
  private $con  = null;//链接资源
   
  /**
   * 初始化Redis
   *
   * @return Object
   */
  public function __construct()
  {
    if ( !class_exists('Redis') )
    {
      throw new QException('PHP extension does not exist: Redis');
    }
    $this->redis = new Redis();
  }
   
  /**
   * 链接memcahce服务
   *
   * @access private
   * @param  string $key 关键字
   * @param  string $value 缓存内容
   * @return array
   */
  private function connect( $sid )
  {
    $file = $this->CacheFile();
    require $file;
    if(! isset($cache) )
    {
      throw new QException('缓存配置文件不存在'.$file);
    }
    $server = $cache[$this->cacheId];
    $sid  = isset($sid) == 0 ? $this->sId : $sid;//memcache服务选择
    if ( ! $server[$sid])
    {
      throw new QException('当前操作的缓存服务器配置文件不存在');
    }
    $host = $server[$sid]['host'];
    $port = $server[$sid]['port'];
    try {
      $this->redis->connect( $host , $port );
    } catch (Exception $e) {
      exit('memecache连接失败，错误信息：'. $e->getMessage());
    }
  }
   
  /**
   * 写入缓存
   *
   * @access private
   * @param  string $key 关键字
   * @param  string $value 缓存内容
   * @return array
   */
  public function set( $key , $value , $sid , $expire = 0)
  {
    $data = $this->get($key , $sid); //如果已经存在key值
    if( $data ) 
    {
      return $this->redis->getset( $key , $value);
    } else {
      return $this->redis->set( $key , $value);
    }
  }
   
  /**
   * 读取缓存
   *
   * @access private
   * @param  string $key 关键字
   * @param  int   $sid 选择第几台memcache服务器
   * @return array
   */
  public function get( $key , $sid)
  {
    $this->connect( $sid );
    return $this->redis->get($key);
  }
   
  /**
   * 清洗（删除）已经存储的所有的元素
   *
   * @access private
   * @return array
   */
  public function flush()
  {
    $this->connect();
    return $this->redis->flushall();
  }
  /**
   * 删除缓存
   *
   * @access private
   * @param  string $key 关键字
   * @param  int   $sid 选择第几台memcache服务器
   * @return array
   */
  public function remove( $key , $sid)
  {
    $this->connect();
    return $this->redis->del($key);
  }
   
  /**
   * 析构函数
   * 最后关闭memcache
   */
  public function __destruct()
  {
    if($this->redis)
    {
      $this->redis->close();
    }
  }
}
```

