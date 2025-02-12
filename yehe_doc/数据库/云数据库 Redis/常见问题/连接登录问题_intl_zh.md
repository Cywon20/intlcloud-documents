### 如何连接 Redis 实例？
云数据库 Redis 支持如下连接方式：通过客户端工具连接、通过数据库管理工具 DMC 连接、通过多语言 SDK 连接。请参见 [连接 Redis 实例](https://intl.cloud.tencent.com/document/product/239/9897)。

### 云数据库 Redis 连接失败，如何处理？
连接失败常见原因有：网络/安全组问题、密码问题以及连接数满了等，对应解决方案请参见 [无法连接 Redis 实例](https://cloud.tencent.com/document/product/239/58020)。

### Redis 支持内网访问的条件有哪些？如何查看内网地址？
内网访问条件：云服务器和数据库须是同一账号，且同一个 VPC 内（保障同一个地域），或同在基础网络内。

登录 [Redis 控制台](https://console.cloud.tencent.com/redis)，在实例列表查看，或单击实例 ID，进入实例详情页查看内网地址。

### 我的云服务器 CVM 和云数据库 Redis，能否直接使用内网连接？
1. 需满足如下条件，才能使用内网连接：
云服务器和数据库须是同一账号，且同一个 VPC 内（保障同一个地域），或同在基础网络内。
2. 是否满足同一个 VPC 内或同在基础网络内，判断方法如下：
 - 云服务器的网络可至 [控制台](https://console.cloud.tencent.com/cvm/instance) 的实例列表或详情页查看。
 - 云数据库 Redis 的网络可至 [控制台](https://console.cloud.tencent.com/redis) 的实例列表或详情页查看。
 详情请参见 [网络类型/ VPC 判断方法](https://cloud.tencent.com/document/product/239/58020#wllxvpdff)。

### 我的云服务器 CVM 和云数据库 Redis，不在同一个 VPC 内，怎么办？
您可以通过云联网来连接实例。
对于不同的 VPC 下（包括同账号/不同账号，同地域/不同地域）的云服务器和数据库，内网连接方式请参见 [云联网](https://cloud.tencent.com/document/product/877)。

### 我的云服务器 CVM 和云数据库 Redis 在不同地域下（如 CVM 在广州，MySQL 在上海），可以直接内网访问吗？
CVM 与 Redis 不在同一地域（地域介绍请参见 [地域和可用区](https://intl.cloud.tencent.com/document/product/239/4106) ）内，属于不同的 VPC 网络，不能直接内网访问，建议在两个 VPC 网络之间建立  [云联网](https://cloud.tencent.com/document/product/877)。

### 我的云服务器 CVM 和云数据库 Redis 在同一个地域下的不同可用区（如 CVM 在上海二区，Redis 在上海一区），可以使用内网连接吗？
云服务器和云数据库 Redis 在同一个地域，不一定是同一个私有网络，也有可能是不同的私有网络。
- 若是同一私有网络的不同可用区，则可以使用内网互联。
- 若不是同一私有网络（如云数据库在 VPC1，云数据库在 VPC2），则无法直接内网互联，解决方案请参见 [更换 Redis 网络](https://intl.cloud.tencent.com/document/product/239/31944)。

### 我的云服务器 CVM 和云数据库 Redis 在同一个私有网络下的不同可用区（如 CVM 在上海二区，Redis 在上海一区），可以使用内网连接吗？
可以使用内网连接，因为同一私有网络下的不同可用区间默认内网互通。

### 不同账号下的云服务器 CVM 和云数据库 Redis ，可以直接内网连接吗？
不同账号下的云服务器 CVM 和云数据库 MySQL，在不同的 VPC 内，不可以直接内网连接，建议使用 [云联网](https://cloud.tencent.com/document/product/877)。

### 如何开通 Redis 的外网访问？ 
请参见 [配置外网地址](https://cloud.tencent.com/document/product/239/63527)。

