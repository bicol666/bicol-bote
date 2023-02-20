# Redis总结

# redis客户端

> Redis客户端比较常用的有三个：jedis、lettuce、redisson



## 1、常用的Redis客户端

| 客户端   | 特点                                                         |
| -------- | ------------------------------------------------------------ |
| Jedis    | 以Redis命令作为方法名称，学习成本低，简单实用。<br>但是Jedis实例是线程不安全的，多线程环境下需要基于连接池来使用 |
| Lettuce  | Lettuce是基于Netty实现的，支持同步、异步和响应式编程方式，并且是线程安全的。<br>支持Redis的哨兵模式、集群模式和管道模式。 |
| Redisson | Redisson是一个基于Redis实现的分布式、可伸缩的Java数据结构集合。<br>包含了诸如Map、Queue、Lock、 Semaphore、AtomicLong等强大功能 |





## 2、jedis

### pom依赖

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.2.3</version>
</dependency>
```



### quick start

```java
@Slf4j
public class TestJedis {

    private Jedis jedis;

    // 在每个测试方法执行前初始化Jedis
    @BeforeEach
    void init() {
        // 创建连接
        this.jedis = new Jedis("139.224.80.235", 6378);
        // 设置密码，没有密码可忽略。
        //jedis.auth("111");
        // 选择库(默认选择的是0)
        jedis.select(0);

    }

    // 在每个测试执行后释放连接
    @AfterEach
    void release() {
        if (!Objects.isNull(jedis)) {
            jedis.close();
        }
    }

    @Test
    void test() {
        // 存String：username-张三
        String result = jedis.set("username", "张三");
        log.info("result:{}",result);
        String username = jedis.get("username");
        log.info("username:{}", username);

        // 获取所有key
        Set<String> keys = jedis.keys("*");
        log.info("所有Key:");
        keys.forEach(TestJedis.log::info);
    }


}

```



### JedisPool

> Jedis本身是线程不安全的，并且频繁的创建和销毁连接会有性能损耗，因此我们推荐大家使用Jedis连接池代替Jedis的直连方式。
>
> Jedis连接池依赖的是`commons-pool`。



`JedisConnectionFactory`

```java
public class JedisConnectionFactory {
    private static final JedisPool jedisPool;

    static {
        // 1、连接池配置对象
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
        jedisPoolConfig.setMaxTotal(8);// 最大连接数
        jedisPoolConfig.setMaxIdle(8);// 最大空闲连接数
        jedisPoolConfig.setMinIdle(2);// 最小空闲连接数
        jedisPoolConfig.setMaxWait(Duration.ofSeconds(2));// 连接池没有可用连接时最大等待时间

        // 2、连接参数
        String host = "139.224.80.235";// 主机IP地址
        int port = 6378;// 端口号
        String password = null; // 密码，若没有设置密码为null
        int timeout = 1000;// 超时时间

        // 3、初始化连接池对象
        jedisPool = new JedisPool(jedisPoolConfig,host,port,timeout,password);
    }

    private JedisConnectionFactory(){}

    // 获取Jedis连接
    public static Jedis getJedis(){
        return jedisPool.getResource();
    }

}

```



`TestJedisPool`

```java
@Slf4j
public class TestJedisPool {

    private Jedis jedis;

    // 在每个测试方法执行前初始化Jedis
    @BeforeEach
    void init() {
        // 创建连接
        this.jedis = JedisConnectionFactory.getJedis();
    }

    // 在每个测试执行后释放连接
    @AfterEach
    void release() {
        if (!Objects.isNull(jedis)) {
            jedis.close();
        }
    }

    @Test
    void test() {
        // 存String：username-张三（测试Jedis连接池）
        jedis.set("username", "张三（测试Jedis连接池）");
        String username = jedis.get("username");
        log.info("username：{}", username);

        // 获取所有key
        Set<String> keys = jedis.keys("*");
        log.info("所有Key:");
        keys.forEach(TestJedisPool.log::info);
    }

}
```



