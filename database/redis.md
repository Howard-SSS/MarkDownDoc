默认16个数据库

端口 6379

日志级别默认 notice(debug\verbose\notice\warning\)

# 特点

客户端单线程多IO(网络连接请求)

# 开启

```java
PATH/redis-server.exe PATH/redis.windows.conf // 开启服务器
PATH/redis-cli -h HOST -p port -a password // 开启客户端
redis-server.exe --service-stop // 停止服务
redis-server.exe --service-start // 重启服务
```

# 配置

```java
config get CONFIG_SETTING_NAME // 获取配置
config set CONFIG_SETTING_NAME NEW_CONFIG_VALUE // 修改配置

```

# 基本

```java
: // 分隔符
set KEY VALUE
get KEY
incr KEY // 加一
decr KEY
incrby KEY NUMBER // 加NUMBER
decrby KEY NUMBER
incrbyfloat KEY NUMBER 
decrbyfloat KEY NUMBER
append KEY VALEU
```

# 数据类型

string、hash、list、set、zset(有序集合)

一个键最大能存储512MB

hash\list\set\存储2^32-1^个键值对

**string**

```java
set KEY VALUE
get KEY
getrange KEY START END // 截取包含边界
```

**hash**

```java
hmset HASH_NAME KEY VALUE [KEY VALUE]
hdel HASH_NAME KEY
hget HASH_NAME KEY
```

**list(如Java LinkedList)**

ziplist + quicklist

```java
lpush LIST_NAME VALUE [VALUE] // 添加到头部
lrange LIST_NAME START END // 从头部Start开始截取到End Start,End >= 0
lpop LIST_NAME // 移除头部元素并获取
rpop LIST_NAME // 移除尾部元素并获取
```

**set**

```java
sadd SET_NAME VALUE
```

**zset**

```java
zadd ZSET_NAME SCORE VALUE // 通过分数排序
```

# HyperLoglog(2.8.9)



# 发布订阅

```java
subscribe CHANNLE_NAME // 订阅频道
publish CHANNLE_NAME TEST // 向频道发送消息
```

# Stream

**优点**

高(性能和内存利用率)，消息持久化存储

**流程**

多个消费者组成消费组争夺一个消息，消费完消费组前移继续争夺下一个消息

# 事务

- 批量操作在发送EXEC命令前被放入队列缓存
- 收到EXEC命令后进入事务执行，事务中任意命令执行失败，**其余的命令依然被执行**
- 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中

```java
multi // 开始事务
set name howard // 命令入队
exec // 执行事务
discard // 取消事务
```

# GEO(3.2)

```java
geoadd KEY_NAME LONGITUDE LATITUDE MEMBER [KEY_NAME LONGITUDE LATITUDE MEMBER] // 添加位置坐标
geopos KEY_NAME MEMBER // 获取位置坐标
geodist KEY_NAME MEMBER MEMBER [m(默认)|km|ft|mi] // 计算两个位置距离
georadius KEY_NAME LONGITUDE LATITUDE RADIUS m|km|ft|mi [withdist|withcoord|withhash|count|asc|desc] // 根据给定地理位置坐标获取指定范围内的地理位置集合
```

