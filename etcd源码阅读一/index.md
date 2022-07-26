# etcd源码阅读(一)


# 原理  
etcd核心是分布式一致性协议算法raft，可以观看[演示动画](http://thesecretlivesofdata.com/raft/)来理解raft算法。
## 集群角色   
raft算法中node的状态可以是跟随者(follower)、候选者(candidate)和领导者(leader)  

## 集群初始化
集群初始化或者leader故障时，最开始的时候所有的节点都是follower状态，且都有一个随机的定时器。  
该定时器按照周期检测leader的心跳信号，如果收到leader的心跳信号，则重置定时器。  
如果超过定时器的时间还没有收到leader的心跳信号，则发起选举，切换成candidate状态，并向集群中其它节点发送请求投票(request vote)的信号。  
其它节点则会在定时器超时的时候进行投票，若candidate收到半数以上(majority)的投票，则选举成功，切换成leader状态。  
以上整个过程被称为leader election。  
## 提交信息
选举成功后，所有提交到集群的变化都要经过leader处理，也就是说，所有节点客户端的请求都会转发到leader节点。  
所有的变化首先将会以条目(entry)的方式，存储到leader节点的日志(log)当中，但此时该条目是未提交(uncommit)的状态。 
leader将会把该条未提交的条目向所有follwer节点发送，当收到半数以上的节点投票以后，leader会将该条目转换成commit状态，并通告(notify)所有的follower该条目变为commit状态。  
以上整个过程被称为log replication。
## 超时时间
第一个是选举超时(election timeout)，是一个介于150~300ms的随机值，超时则节点进入candidate状态，进入新的选举周期(election term)，请求其它节点投票，并将自己的票投给自己。  
如果收到投票请求的节点在这个新的选举周期还没有投过票，则会将票投给请求的节点，并重置选举超时时间。  
candidate变成leader后会定时发送附加条目信息(Append Entries messages)，其定时时间则是心跳超时时间(heartbeat timeout),follower会对每一条附加条目信息进行回复。
## 超时重新选举
如果leader发送的间隔超过follower选举超时时间，则follower会变成candidate并开始新的选举周期重新选举。
## 特殊情况
若两个candidate都得到了相同的票数，则不会选出leader，并等待下一个选举周期重新选举。
## 变化的同步
通过用于心跳时间的附加条目信息一同发送到其它节点。
从客户端写到集群的过程可以描述为：
客户端发送请求Set-->leader写log,此时是未提交状态-->follower接受通知，并有半数以上投票-->leader修改条目为提交状态-->通知客户端和follower成功写入
## 集群分区情况
在出现集群分区的情况时（集群节点由于某些原因被分到两个集群即脑裂情况），则节点数较少的集群由于投票数不会多于半数其修改都是未提交的状态，选举周期也不会变，节点数较多的可以正常工作，当整个集群恢复正常的时候，之前节点数较少集群的未提交修改变化会回退，这些节点由于选举周期低于大多数的集群则会退回follower的状态加入多数节点的集群，并且log会随之更新。
