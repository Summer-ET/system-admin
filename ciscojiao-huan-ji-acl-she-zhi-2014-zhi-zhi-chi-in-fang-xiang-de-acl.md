# Cisco交换机如何设置只支持In方向的ACL



ACL：Access Control List，访问控制列表，是路由器和交换机接口的指令列表，用来控制端口进出的数据包

有三种主要的ACL:标准ACL、扩展ACL及命名ACL

标准的ACL使用1 ~ 99以及1300~1999之间的数字作为表号

标准ACL一般只能过滤以IP为标的流量进出

扩展的ACL使用100 ~ 199以及2000~2699之间的数字作为表号

扩展ACL可以使用协议，端口，IP等组合来使用过滤流量



在路由器和三层交换机上，ACL可以应用在端口的进和出两个方向上，但部分二层交换机只能设置进方向上的ACL，怎么办呢？

如：

二层交换机Cisco 2960的1号是连internet，而10号口连接了web服务器

```
config t

access-list 101 permit tcp any any eq 80
int gi 1/0/1
ip access-group 101 in ----此交换机只支持在in方向设置ACL
exit
```

此时在1号口应用上述的ACL只能达到外部访问web服务器，而web服务器所有的出去的流量比如访问位于外部的ftp服务器，都将被阻止，怎么办呢？

其实添加一条如下的ACL，同样应用在1号口上即可：

```
access-list 101 permit tcp any any eq 80
access-list 101 permit tcp any eq 21 any
```

**总结**：

虽然只能设置in方向的ACL，但交换机的判断只要是返回包是匹配的即可，并不判断发起请求的方向是否匹配。

 

