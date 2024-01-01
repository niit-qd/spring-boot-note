
---
### redis server的使用

1. 启动redis
    ``` shell
    redis-server /etc/redis.conf
    ```
2. 外网访问
    修改`/etc/redis.conf`，去掉`bind`配置，或者添加`bind * -::*`。
    ``` conf
    ################################## NETWORK #####################################

    # By default, if no "bind" configuration directive is specified, Redis listens
    # for connections from all available network interfaces on the host machine.
    # It is possible to listen to just one or multiple selected interfaces using
    # the "bind" configuration directive, followed by one or more IP addresses.
    # Each address can be prefixed by "-", which means that redis will not fail to
    # start if the address is not available. Being not available only refers to
    # addresses that does not correspond to any network interface. Addresses that
    # are already in use will always fail, and unsupported protocols will always BE
    # silently skipped.
    #
    # Examples:
    #
    # bind 192.168.1.100 10.0.0.1     # listens on two specific IPv4 addresses
    # bind 127.0.0.1 ::1              # listens on loopback IPv4 and IPv6
    # bind * -::*                     # like the default, all available interfaces
    #
    # ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
    # internet, binding to all the interfaces is dangerous and will expose the
    # instance to everybody on the internet. So by default we uncomment the
    # following bind directive, that will force Redis to listen only on the
    # IPv4 and IPv6 (if available) loopback interface addresses (this means Redis
    # will only be able to accept client connections from the same host that it is
    # running on).
    #
    # IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
    # COMMENT OUT THE FOLLOWING LINE.
    #
    # You will also need to set a password unless you explicitly disable protected
    # mode.
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    # bind 127.0.0.1 -::1
    bind * -::*

    ```

3. 无密码访问
    修改`protected-mode`的值为`no`。
    ``` conf
    # Protected mode is a layer of security protection, in order to avoid that
    # Redis instances left open on the internet are accessed and exploited.
    #
    # When protected mode is on and the default user has no password, the server
    # only accepts local connections from the IPv4 address (127.0.0.1), IPv6 address
    # (::1) or Unix domain sockets.
    #
    # By default protected mode is enabled. You should disable it only if
    # you are sure you want clients from other hosts to connect to Redis
    # even if no authentication is configured.
    # protected-mode yes
    protected-mode no
    ```
4. 


---
### SpringBoot 相关

1. 自动配置
   `org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration`。
2. 模板
    - `RedisTemplate<Object, Object> redisTemplate`
        引入的时候，必须去掉泛型，或者是`StringRedisTemplate stringRedisTemplate`。
        指定具体泛型类型的写法是不正确的，例如：`RedisTemplate<String, String> redisTemplate`
    - `StringRedisTemplate stringRedisTemplate`
    <br/>
    `redisTemplate`和`stringRedisTemplate`这两个模板在处理的key的时候，这些项无法相互读取，例如下面
    ``` java
    ValueOperations<Object, Object>  valueOperations = redisTemplate.opsForValue();
    valueOperations.set("test", "" + System.currentTimeMillis());
    logger.info("get value:{}, {}", "test", valueOperations.get("test"));

    ValueOperations<String,String> stringValueOperations = stringRedisTemplate.opsForValue();
    stringValueOperations.set("test_string", "" + System.currentTimeMillis());
    logger.info("get value:{}, {}", "test_string", stringValueOperations.get("test_string"));

    // 下面的取值就是null：
    logger.info("get value:{}, {}", "test_string", valueOperations.get("test_string"));
    logger.info("get value:{}, {}", "test", stringValueOperations.get("test"));
    ```
    通过使用[AnotherRedisDesktopManager](https://github.com/qishibo/AnotherRedisDesktopManager)工具查看，`redisTemplate`是以二进制形式进行读写的。
3. 使用Jedis
    SpringBoot3使用`Lettuce`作为Redis的客户端，如果要使用`Jedis`，如下配置：
    可以不用注释掉`lettuce-core`。
    - pom.xml
        ``` xml
                <dependency>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-data-redis</artifactId>
        <!--            <exclusions>-->
        <!--                <exclusion>-->
        <!--                    <groupId>io.lettuce</groupId>-->
        <!--                    <artifactId>lettuce-core</artifactId>-->
        <!--                </exclusion>-->
        <!--            </exclusions>-->
                </dependency>
                <!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
                <dependency>
                    <groupId>redis.clients</groupId>
                    <artifactId>jedis</artifactId>
                    <version>4.4.6</version>
                </dependency>
        ```
    - application.yml
        ``` yaml
        spring:
        data:
            redis:
            host: x.x.x.x
            port: 6379
            database: 0
            username:
            password:
            client-type: jedis
        ```
    
    注意：不要使用`Jedis » 5.x.x`，否则报异常：`java.lang.NoClassDefFoundError: redis/clients/jedis/Queable`。
    解决方案：暂无。
4. 