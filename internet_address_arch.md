# IP 地址相关总结

### 介绍

对于 IPv4 地址通常用点分十进制表示，例如：165.195.130.107 由4个十进制整数组成并且用点号分隔，由于每个十进制数对应4个bit, 所以十进制的取值范围为0-255，具体点分十进制到二进制转化可以用http://www.subnetmask.info/ 转化。

例如点分十进制于二进制对应表:

| 点分十进制	  | 二进制								|
| --------------- | ------------------------------------|
| 0.0.0.0		  | 00000000 00000000 00000000 00000000 |
| 1.2.3.4		  | 00000001 00000010 00000011 00000100 |
| 10.0.0.255	  | 00001010 00000000 00000000 11111111 |
| 165.195.130.107 | 10100101 11000011 10000010 01101011 |
| 255.255.255.255 | 11111111 11111111 11111111 11111111 |

对于 IPv6 是由128 bit 位表示，长度是 IPv4 的4被，IPv6 一般是由8个4位的16进制数组成中间用冒号分隔，例如：5f05:2000:80ad:5800:0068:0800:2023:1d71, 如下是表示IPv6的规则：

- 每块以0开头的可以省略不写，如前面的地址可以写作为：5f05:2000:80ad:5800:68:800:2023:1d71
- 如果分组块都是0，可以省略并且用符号(::) 替换，例如：地址 0:0:0:0:0:0:0:1 可以简写为 ::1, 2001:0db8:0:0:0:0:0:2 可以简写为 2001:db8::2, 为了避免歧义，(::)符号只能在整个IP地址中出现一次
- 嵌入IPv4地址表示IPv6的方法是通过在IPv4地址前加上 ffff 混合表示的一种方式，IPv4 地址部分继续用点分十进制表示，例如： ::ffff:10.0.0.1 表示IPv4地址 10.0.0.1, 称为 IPv4到IPv6的映射
- 另一个表示法是 IPv6 的 32低bit位可以用点分十进制表示，例如 ::0102:f001 等同于 ::1.2.240.1，该种表示法称为IPv6 兼容 IPv4 的一种方式，这种方法在 IPv6初期很常见，现在已经不再需要了

在某些情况下（如URL地址中包含IPv6时）IPv6 地址中的分号分隔符可能会和其他标识符冲突，例如ip 地址和端口号中间的分隔符也是分号，在这种情况下需要将IP地址用中括号括起来，例如：

http://[2001:0db8:0:0:0:0:0:2]:443/

### IP 地址结构基础

#### 地址分类

最初定义 IP 地址结构的时候是将每个常规IP地址分为网络部分和主机部分，其中网络部分是用来查找所属的网络，主机部分用来标识在该网络中唯一的主机，一些连续的bit位组成一个网络号，剩下的bit位组成主机号

如图，IP 地址被分为A,B,C,D,E 五个类型，其中A,B,C属于常规IP类型，每个类型对应一个IP段范围：


| 类型 | 范围						 | 高位 Bit 位 | 用途			 | 占比 | 网络数  | 主机数   |
| ---- | --------------------------- | ----------- | --------------- | ---- | ------- | -------- |
| A    | 0.0.0.0 - 127.255.255.255   | 0		   | Uncast/特殊地址 | 1/2  | 128     | 16777216 |
| B	   | 128.0.0.0 - 191.255.255.255 | 10		   | Uncast/特殊地址 | 1/4  | 16384   | 65536    |
| C	   | 192.0.0.0 - 223.255.255.255 | 110  	   | Uncast/特殊地址 | 1/8  | 2097152 | 256      |
| D	   | 224.0.0.0 - 239.255.255.255 | 1110  	   | 广播地址		 | 1/16 | N/A	  | N/A      |
| D	   | 240.0.0.0 - 255.255.255.255 | 1111  	   | 保留			 | 1/16 | N/A	  | N/A      |

上表说明例如一个网络号为 18.0.0.0 的A类网络，可以分配 2^24 个主机地址(18.0.0.0-18.255.255.255), 但是由于网络号占8bit，所以最大可以分配127个A类网络在整个Internet中，一个C类网络地址，如192.125.3.0 最多可以分配 256个主机(192.125.3.0-192.125.3.255), 但是C类网络号可以分配200多万个，随着互联网的发展通过对IP地址分类分配逐渐遇到可扩展的问题，比如分配一个A类或B类地址将浪费很多主机地址，然而C类地址主机地址又不够用的问题。


### 子网地址


