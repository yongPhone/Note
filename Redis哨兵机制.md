## 在线状态监测

哨兵有两种判断用户在线的方法，主观和客观方法。

- 主观：Redis 服务器的在线判断依据是某个哨兵自己的信息
- 客观：Redis 服务器的在线判断依据是由其他监视此 Redis 服务器的哨兵的信息。

![img](http://wiki.jikexueyuan.com/project/redis/images/a4.png)

主观下线：哨兵凭借的自己的信息判断 Redis 服务器是否下线的方法，称为主观方法，即通过判断前面有提到的 PING 心跳等其他通信时间是否超时来判断主机是否下线。主观的信息有可能是错的。

![img](http://wiki.jikexueyuan.com/project/redis/images/a5.png)

客观下线：哨兵通过所有其他哨兵报告的主机在线状态来判定某主机是否下线

## 故障修复

哨兵如果检测主机被大多数的哨兵判定为下线，就很可能会执行故障修复，重新选出一个主机。

故障修复分成了几个步骤完成，每个步骤对应一个状态。

![img](http://wiki.jikexueyuan.com/project/redis/images/b.png)



### WAIT_START

**目的：在哨兵中选出首领**

在哨兵服务器群中，有首领的概念，这个首领可以是系统管理员根据具体情况指定的，也可以是众多的哨兵中按一定的条件选出的。在 WAIT_STATE 中执行故障修复的哨兵首先确定自己是不是首领，如果不是故障修复会被拖延，到下一个定时程序再次检测自己是否为首领，超过一定时间会强制停止故障修复。

**怎么样才可以当选一个首领呢？**每一个哨兵都会有一个当前的配置版本号 current_-epoch，每一个哨兵手里都会有一票投给其中一个配置版本最高的哨兵，它的投票信息将会通过 is-master-down 命令交换。is-master-down 命令在故障修复的时候会被强制触发，收到它的哨兵将会进行投票并返回自己的投票结果，如此一来，执行故障修复的哨兵就能得到其他哨兵的投票结果，它就能确定自己是不是首领了。

### SELECT_SLAVE

**目的：选出一台候选主机**

SELECT_SLAVE 的意图很明确，因为当前的主机（master）已经挂了，需要重新指定一个主机，候选的服务器就是当前挂掉主机的所有从机（slave）。

1. 当前执行故障修复的哨兵会遍历主机的所有从机，只有足够健康的从机才能被成为候选主机。

2. 如果经过筛选之后有多台从机，那么这些从机会按下面的条件排序：

   1. 从服务器按照 Redis 实例的 redis.conf 文件中配置的 slave-priority 排序。更低的优先级更偏爱。

      ```properties
      # The slave priority is an integer number published by Redis in the INFO output.
      # It is used by Redis Sentinel in order to select a slave to promote into a
      # master if the master is no longer working correctly.
      #
      # A slave with a low priority number is considered better for promotion, so
      # for instance if there are three slaves with priority 10, 100, 25 Sentinel will
      # pick the one with priority 10, that is the lowest.
      #
      # However a special priority of 0 marks the slave as not able to perform the
      # role of master, so a slave with priority of 0 will never be selected by
      # Redis Sentinel for promotion.
      #
      # By default the priority is 100.
      slave-priority 100
      ```

   2. 优先选择主从复制偏移量高的从机，即从机从主机复制的数据越多

   3. 优先选择有 run-id 的从机

   4. 如果上面条件都一样，那么将 run-id 按字典顺序排序

### SLAVEOF_NOONE

**目的：让候选主机转变成主机**

我们知道，slaveof noone 命令可以让一个从机转变为一个主机，Redis 从机收到会做从从机到主机的转换。发送 slaveof noone 命令之后，哨兵还会向候选主机发送 config rewrite 让候选主机当前配置信息写入配置文件，以方便候选从机下次重启的时候可以恢复。

![img](http://wiki.jikexueyuan.com/project/redis/images/b2.png)

### WAIT_PROMOTION

这一状态纯粹是为了等待上一个状态的执行结果（如候选主机的一些状态）被传播到此哨兵上

### RECONF_SLAVES

**目的：让从机连接新主机**

这一状态主要做的是向其他非候选从机发送 slaveof promote_slave，即让候选主机成为他们的主机。其中会涉及几个 Redis 服务器状态的标记：SRI_RECONF_SENT，SRI_RECONF*INPROG，SRI*-RECONF_DONE，分别表示已经向从机发送 slaveof 命令，从机正在重新配置（这里需要一些时间），配置完成。同样，哨兵是通过 INFO 数据传输中获知这些状态变更的。

![img](http://wiki.jikexueyuan.com/project/redis/images/b3.png)

### UPDATE_CONFIG

这里还存在最后一个状态 UPDATE_CONFIG。因为主机和从机发生了修改，所以 sentinel.masters 肯定需要修改，譬如主机的IP 地址和端口，所以最后的工作是将修改并整理哨兵服务器保存的信息。

![img](http://wiki.jikexueyuan.com/project/redis/images/b4.png)



还有一个问题：故障修复过程中，一直没有发送 SLAVEOF promoted_slave 给旧的主机，因为已经和旧的主机断开连接，哨兵没有选择在故障修复的时候向它发送任何的数据。但在故障修复的最后一个状态中，哨兵依旧有将旧的主机塞到新主机的从机列表中，所以哨兵还是会超时发送 INFO HELLO 等数据，对旧的主机抱有希望。如果因为网络环境的不佳导致的故障修复，那旧的主机很可能恢复过来，只是这时它是一台从机了。