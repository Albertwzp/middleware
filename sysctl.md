# Controls IP packet forwarding
net.ipv4.ip_forward = 0

# Do not accept source routing
net.ipv4.conf.default.accept_source_route = 0

# Controls whether core dumps will append the PID to the core filename.
# Useful for debugging multi-threaded applications.
kernel.core_uses_pid = 1

# Controls the use of TCP syncookies
net.ipv4.tcp_syncookies = 1

# Controls the maximum size of a message, in bytes
kernel.msgmnb = 65536

# Controls the default maxmimum size of a mesage queue
kernel.msgmax = 65536

net.ipv4.conf.all.promote_secondaries = 1
net.ipv4.conf.default.promote_secondaries = 1
net.ipv6.neigh.default.gc_thresh3 = 4096 
net.ipv4.neigh.default.gc_thresh3 = 4096

kernel.softlockup_panic = 1
kernel.numa_balancing = 0
kernel.shmmax = 68719476736
kernel.printk = 5


# 禁用ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1

# swap使用规则
vm.swappiness = 0

# #决定检查过期多久邻居条目
net.ipv4.neigh.default.gc_stale_time=120

# 修改为0；反向过滤技术，系统在接收到一个IP地址发来的包后，会检查如果自己发送包到该IP时应该使用的网口是否和接收该IP地址发来的包的网口是同一个网口，如果不是，则该包会被抛弃，通常多网卡时考虑是否使用
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.all.rp_filter = 0
net.ipv4.conf.eth0.rp_filter = 0

# 查看https://www.jianshu.com/p/a682ecae9693，腾讯云默认为0，写在这里是因为阿里云配置为2，改变主机拥有多地址时相应arp包的行为，暂时不改变
net.ipv4.conf.default.arp_announce = 0
net.ipv4.conf.lo.arp_announce = 0
net.ipv4.conf.all.arp_announce = 0

# 控制socket重用，注意：在MQTT类型的服务所在的主机上，不能开启reuse和recycle
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 0

# 开启了tcp_tw_recycle之后，会检查同一源IP主机发过来的包的时间戳，如果60s内的时间戳乱序了，则时间戳滞后的包会被抛弃，所以开启tw_recycle后要将该参数设为0，避免出现无响应的情况
net.ipv4.tcp_timestamps = 1

# 等待fin包的时间
net.ipv4.tcp_fin_timeout = 15

# 系统建立TCP连接的超时重试次数（不含首次连接请求），每次重试都会等待2的n（n从0开始）次方后进行第二次重试，即第一次连接请求后等待1s发起第二次请求（第一次重试），然后是2s，4s，8s等等
# 改为3，即一共发起4次连接，7s建立不了连接则返回失败
net.ipv4.tcp_synack_retries = 3

# 定义系统中每一个端口最大的监听队列的长度，虽然服务器运行的是单一应用，默认的128也太小了
net.core.somaxconn = 32768

# 定义尚未收到客户端确认信息的连接请求的最大值
net.ipv4.tcp_max_syn_backlog = 10240

# 定义处于TIME-WAIT状态的最大连接数
net.ipv4.tcp_max_tw_buckets = 5000

# 每个网络接口接收数据包的速率比内核处理这些包的速率快时，允许送到队列的数据包的最大数目
net.core.netdev_max_backlog = 8192

# 表示用于向外连接的端口范围。默认：32768到61000，改为1024到65000
net.ipv4.ip_local_port_range = 1024 65535

# 接收套接字缓冲区大小的默认值(以字节为单位)
net.core.rmem_default = 262144

# 发送套接字缓冲区大小的默认值(以字节为单位)
net.core.wmem_default = 262144

# 接收套接字缓冲区大小的最大值(以字节为单位)16M
net.core.rmem_max = 16777216

# 发送套接字缓冲区大小的最大值(以字节为单位)16M
net.core.wmem_max = 16777216

# 每个socket的副缓冲区大小 16M
net.core.optmem_max = 16777216

# 确定TCP栈应该如何反映内存使用，每个值的单位都是内存页（通常是4KB),根据服务器硬件配置设为：low:0.5G,pressure:1G,higth:1.5G
# low:当TCP使用了低于该值的内存页面数时，TCP不会考虑释放内存.
# pressure：当TCP使用了超过该值的内存页面数量时，TCP试图稳定其内存使用，进入pressure模式，当内存消耗低于low值时则退出pressure状态
# 允许所有tcp sockets用于排队缓冲数据报的页面量，当内存占用超过此值，系统拒绝分配socket，后台日志输出“TCP: too many of orphaned sockets”。
net.ipv4.tcp_mem = 131072 262144 393216

# TCP读buffer
net.ipv4.tcp_rmem = 1024 4096 16777216

# TCP写buffer 
net.ipv4.tcp_wmem = 1024 4096 16777216

# 该参数决定了系统中所允许的文件句柄最大数目，文件句柄设置代表linux系统中可以打开的文件的数量
fs.file-max = 2097152

# 单个进程可分配的最大文件数
fs.nr_open = 2097152

# 这个参数表示当keepalive启用时，TCP发送keepalive消息的频度。默认是2小时，若将其设置得小一些，可以更快地清理无效的连接，MQTT类型服务所在云主机慎用
net.ipv4.tcp_keepalive_time = 1800

# TCP 发送 keepalive的次数,用于确认 tcp 链接是否有效
net.ipv4.tcp_keepalive_probes = 6

# 探测消息未获得响应，重试间隔时间。
net.ipv4.tcp_keepalive_intvl = 15

# 表示系统中最多有多少TCP套接字不被关联到任何一个用户文件句柄上。如果超过这里设置的数字，连接就会复位并输出警告信息。
# 这个限制仅能防止简单的DoS攻击。此值不能太小，但增大该值会增加内存的消耗。默认65536
net.ipv4.tcp_max_orphans = 100000

# 调整刷盘策略，应用服务器用于跑应用程序，主要存盘的数据是日志，日志可以丢失，也不需要很强的实时刷盘，修改两个参数，其他保持默认
# 脏数据达到5%就开始异步刷盘
vm.dirty_background_ratio = 5

# 脏数据达到60%之前都不会触发强制同步刷盘
vm.dirty_ratio = 60
kernel.sysrq = 1

[sysctl](https://blog.slogra.com/post-672.html)
