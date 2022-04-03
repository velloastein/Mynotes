redis 键操作：

keys *

 del key

unlink key

type key 

expire key 设置过期时间

ttl key 查看过期时间 -1表示永不过期 -2表示已过期

select 切换数据库

dbsize 查看当前库的key数量

flushdb清空当前库

flushall通杀全部库

persist 移除过期时间



set key value

get key  返回key的值

append  key value 追加一个值到key上

strlen key 获取指定key的值的长度

setnx key value 设置一个key的值（只有当该key不存在才可以）

incr key 执行原子加1操作

decr 执行原子减一操作

incrby key incremwnt 执行原子增加一个整数

mset key value key value 设置多个key value值

mget key key 获取多个key的值

getrange key  start end 获取存储在key上的值的子字符串

setrange key offset value 从指定的offset处开始覆盖value的长度

setex key seconds value 设置key value并设置过期时间

getset key value 设置一个key value，并获取设置前的值



列表底层是个双向链表，单键多值

lpush 左re边加入（头插）

rpush 右边加入（尾插）

lpop

rpop

rpoplpush 从右边取值放到左边

lrange 从左到右按照索引获取列表元素

lindex 按照索引下标获得元素

llen 获取列表长度

linsert before

linsert after

lrem 从左边删除

lset 替换值



quicklist 由多个压缩链表构成



集合底层是个哈希表

sadd 

smenmbers

sismember

scard 返回元素个数

srem 删除某个元素

spop 随机弹出一个元素

srandmember 随机取出n个值，但不会删除

smove  从一个集合移到另一个集合

sinter 取交集

sunion 取并集

sdiff 取差集



Hash底层是个hashmap

hset 

hget

hmset

hexists

hkeys

hvals

hincrby

hsetnx



有序集合Zset

zadd

zrange

zrange withscores

zrangebyscore

zrevrangebyscore 从大到小

zincreby

zrem

zcount

zrank



位图

setbit

getbit

bitcount

bitop 



HyperLogLog

pfcount

pfadd

pfmerge



Geospatial

geoadd

geodist

geopos



redis事务

multi 组队

discard 放弃组队

exec 执行阶段

watch

组队过程出现错误，所有命令不会成功

组队成功，执行中失败的命令不会执行

悲观锁



事务特性：

单独的隔离操作

没有隔离级别

不保证原子性



持久化:
RDB:最后一次持久化会造成数据丢失

AOF：

RDB和AOF同时开启。默认取AOF数据

主从复制的好处：

1.读写分离

2.容灾的快速恢复



创建myredis文件夹

复制redis.conf

配置一主两从

在三个配置文件中写入内容

启动三个redis服务 slaveof 主机号



services.msc

启动客户端：redis-cli -p 6379

redis-server --service-install redis.windows.conf

启动服务：redis-server --service-start

关闭服务：redis-server --service-stop

卸载：redis-server --service-uninstall 

redis-server --service-install redis.windows.conf --loglevel verbose  --service-name 服务名称



从服务器连上主服务器，会发生一个同步的消息

主服务器收到消息，会先RDB,再把rdb文件发给从服务器 （全量复制）

主服务器中写操作后，会同步给从服务器 （增量复制）

每次重连会发生全量复制



slave no one 反客为主

sentinel monitor mymaster 127.0.0.1 6379 1



哨兵模式：

优先级高的 peplica-preority

偏移量大的



集群：

cluster-enabled

16384插槽

组

cluster keyslot

cluster  countkeysslot



好处：

扩容

分摊压力

无中心配置



缓存穿透：

现象：

-应用服务器压力变大了

-redis命中率降低

-一直查询数据库



缓存穿透：

redis查询不到数据库

出现很多非正常url访问



解决方案：

对空值缓存

设置白名单

采用布隆过滤器

进行实时监控



缓存雪崩：

现象：数据库访问压力瞬时增加

redis没有出现大量key过期

redis正常运行

原因：

redis某个key过期，大量访问使用这个key

解决方案：

预先设置热门数据

实时调整

使用锁



缓存雪崩：

现象：数据库压力变大服务器崩溃

原因：极少时间内，查询大量key集中过期

解决方案：

构建多级缓存架构

使用锁和队列

设置过期标志提前更新缓存

让缓存失效时间分散开



分布式锁：

跨jvm的互斥

setnx del

uuid

