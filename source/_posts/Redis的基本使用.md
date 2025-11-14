---
title: Redis的基本使用
---





#### 基本命令(包含centos命令和redis的命令)

后面的是redis的命令

| 命令/示例                    | 作用                                                  |
| ---------------------------- | ----------------------------------------------------- |
| redis-server                 | 前台启动redis服务                                     |
| redis-server redis.conf目录  | 后台启动redis服务                                     |
| redis-server /etc/redis.conf | 后台启动redis服务                                     |
| >ping                        | 返回PONG表示redis连接成功                             |
| ps -ef \| grep redis         | 显示redis的所有进程                                   |
| kill -9 ID                   | 杀死 ID的进程                                         |
| redis-cli -p 6379            | 用6379端口启动redis,默认使用6379端口可以不用写-p 6379 |
| redis-cli shutdown           | 单实例redis关闭                                       |
| >shutdown                    | 关闭进入的redis实例                                   |
| redis-cli -p 6379 shutdown   | 多redis实例关闭，指定端口关闭                         |



#### 常用命令

| 命令/示例                        | 作用                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| set key value                    | 添加键值对                                                   |
| get key                          | 查询对应的键值                                               |
| append key value                 | 将给定的value 追加到原值的末尾                               |
| strlen key                       | 获得值的长度                                                 |
| setnx key value                  | 只有在key不存在时，设置key的值                               |
| incr key                         | 将key中存储的数字值增加1，只能对数字值操作，如果为空，值设置为1 |
| decr key                         | 将key中存储的数字值减少1，只能对数字值操作，如果为空，值设置为-1 |
| incrby/decrby key> 步长          | 将 key 中储存的数字值增减步长值，步长可以为负数              |
| mset key1 value1 key2 value2 ... | 同时设置一个或多个key-value对                                |
| mget key2 value2 key2 value2     | 同时获取一个或多个value                                      |
| msetnx key1 value1 key2 value2   | 同时设置一个或多个key-value对，当且仅当所有给定key都不存在   |
| getrange key 起始位置 结束位置   | 获得值的范围，类似java中的substring，前包，后包              |
| setrange key 起始位置 value      | 用value 覆写key所存储的字符串值，从起始位置开始(索引从0开始) |
| set key 过期时间 value           | 设置键值的同时，设置过期时间，单位秒                         |
| getset key value                 | 依旧换新，设置新值同时输出旧值                               |



Redis列表(List)

| 命令/示例                                 | 作用                                              |
| ----------------------------------------- | ------------------------------------------------- |
| lpush/rpush  key1 value1 key2 value2  ... | 从左边/右边插入一个或多个值                       |
| lpop/rpop key                             | 从左边/右边吐出一个值。值在键在，值光键亡         |
| rpop/lpush key1 key2                      | 从key1列表右边吐出一个值，插到key2列表左边        |
| lrange key start stop                     | 按照索引下标获得元素(从左到右)                    |
| lrange mylist 0 -1                        | 0左边第一个，-1右边第一个<br />改命令表示获得所有 |
| lindex key index                          | 按照索引下标获得元素(从左到右)                    |
| llen key                                  | 获得列表长度                                      |
| linsert key before value newvalue         | 在value前面插入newvalue                           |
| lrem key n value                          | 从左边删除n个value(从左到右)                      |
| lset key index value                      | 将列表key下标为index的值替换成value               |



Redis集合(set)

| 命令/示例                      | 作用                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| sadd key value1 value2         | 将一个或多个member元素加入到集合key中，已经存在的member元素将被忽略 |
| smembers key                   | 取出该集合的所有值                                           |
| sismember key value            | 判断集合key是否为含有该value值，有1，没有0                   |
| scard key                      | 返回该集合的元素个数                                         |
| srem key value1 value2 ...     | 删除集合中的某个元素                                         |
| spop key                       | 随机从该集合中吐出一个值                                     |
| srandmember key n              | 随机从该集合中取出n个值，不会从集合中删除                    |
| smove source destination value | 把集合中一个值从一个集合移动到另一个集合                     |
| sinnter key1 key2              | 返回两个集合的交集元素                                       |
| sunion key1 key2               | 返回两个集合的并集元素                                       |
| sdiff key1 key2                | 返回两个集合的差集元素(key1中的，不包含key2中的)             |



#### Redis哈希(Hash)

| 命令/示例                                 | 作用                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| hset key field value                      | 给key集合中的field键赋值value                                |
| hget key1 field                           | 从key1集合field取出value                                     |
| hmset key1 field1 value1 field2 value2... | 批量设置hash的值                                             |
| hexists <key1 field                       | 查看哈希表key中，给定域field是否存在                         |
| hkeys key                                 | 列出该hash集合的所有field                                    |
| hvals key                                 | 列出该hash集合的所有value                                    |
| hincrby key field increment               | 为哈希表key中的域field的值加上增量increment                  |
| hsetnx key field value                    | 将哈希表key中的域field的值设置为value，当且仅当域field不存在 |



#### Redis有序集合Zset(sorted set)

主要是根据score来排序

| 命令/示例                                                  | 作用                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| zadd key score1 value1 score2 value2 ...                   | 将一个或多个member元素及其score值加入到有序key当中           |
| zrange key start stop [WITHSCORES]                         | 返回有序集key中，下标在start stop 之间的元素<br />带WITHSCORES可以让分数一起和值返回到结果集 |
| zrangebyscore key minmax [withscores] [limit offset count] | 返回有序集key中，所有score值介于min和max之间(包括等于min或max)的成员，有序集成员按score值递增(从小到大)次序排列 |
| zrevrangebyscore                                           | 同上，改为从大到小排列                                       |
| zincrby key increment value                                | 为元素的score加上增量                                        |
| zrem key value                                             | 删除该集合下指定值的元素                                     |
| zcount key min max                                         | 统计该集合，分数区间内的元素个数                             |
| zrank key value                                            | 返回该值在集合中的排名，从0开始                              |



#### Redis的一些相关配置

配置只有保存后，并重启查看进程才能生效

```
> shutdown
redis-server /redis.conf的路径
```

注释掉bind 127.0.0.1

```
# bind 127.0.0.1
```

将本机的访问保护模式设置no

```
protected-mode no
```

端口

```
port 6379
```

设置为后台进程

```
daemonize yes
```

设置密码

```
requirepass foobared #将foobared改为自己的密码，去掉注释就行
# 之后使用redis-cli登录redis后使用命令：auth 你的密码
# 这样就可以密码登录了 或者redis-cli -a 你的密码
```



#### Redis的发布和订阅

| 命令/示例              | 作用                          |
| ---------------------- | ----------------------------- |
| subscribe channel1     | 需要先进入redis，订阅channel1 |
| publish channel1 hello | 给channel1频道发信息          |



#### 新数据类型

##### Bitmaps

| 命令/示例                           | 作用                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| setbit key offset value             | 设置Bitmaps中某个偏移量的值（0或1）                          |
| getbit key offset                   | 获取Bitmaps中某个偏移量的值                                  |
| bitcount key [start end]            | 统计字符串从start字节到end字节比特值为1的数量                |
| bitop and(or/not/xor) destkey [key] | bitop是一个符合操作，他可以做多个Bitmaps的and(交集)、or(并集)、not(非)、xor(异或)操作并将结果保存在destkey中 |



##### HyperLogLog

| 命令/示例                                | 作用                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| pfadd key element [element ...]          | 添加指定元素到HyperLogLog中                                  |
| pfcount key [key...]                     | 计算key的近似基数                                            |
| pfmerge destkey sourcekey [sourcekey...] | 将一个或多个key合并后的结果存在另一个key中，比如每月活跃用户可以使用每天的活跃用户来合并计算得到 |



##### Geospatial

| 命令/示例                                                    | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| geoadd key logitude latitude member [longitude latitude member ...] | 添加地理位置(经度，纬度，名称)                               |
| geopos key member [member...]                                | 获得指定地区的坐标值                                         |
| geodis key member1 member2 [m\|km\|ft\|mi]                   | 获取两个位置之间的直线距离<br />(m:米(默认值),km:千米,mi:英里,ft英尺) |
| georadius key longitude latitude radius m\|km\|ft\|mi        | 以给定的经纬度为中心， 找出某一半径内的元素（经度 纬度 距离 单位） |



#### Java连接Redis

```java
import redis.clients.jedis.Jedis;
public class Demo01 {
	public static void main(String[] args) {
		Jedis jedis = new Jedis("192.168.137.3",6379);
		String pong = jedis.ping();
		System.out.println("连接成功："+pong);
		jedis.close();
    }
}
```



#### Redis\_事务

| 命令/示例 | 作用                                                         |
| --------- | ------------------------------------------------------------ |
| multi     | 从输入 Multi 命令开始，输入的命令都会依次进入命令队列中，但不会执行，直到输入 Exec 后，Redis 会将之前的命令队列中的命令依次执行。 |
| exec      | 同上                                                         |
| discard   | 组队的过程中可以通过 discard 来放弃组队                      |



#### 备注

后面还有持久化-RDB和AOF，用的时候查一下“尚硅谷的笔记”或者百度吧，暂时没练过，不知道记录啥，本人还是主要想去学一下C#和unity，要不是大数据作业，估计不会看这个[-_-||]

主从复制

lua脚本

集群

- 缓存穿透
- 缓存击穿
- 缓存雪崩




#### 参考视频

[参考B站视频](https://www.bilibili.com/video/BV1Rv41177Af "B站尚硅谷Redis学习视频")