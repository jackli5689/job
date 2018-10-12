#Zabbix

yum install -y lrzsz   	#安装secretCRT中的rz上传和sz下载
####监控概述

######监控对象
1. 监控对象的理解：CPU是怎么工作的。原理
2. 监控对象的指标：CPU使用率， CPU负载  CPU个数  上下文切换
3. 确定性能基准线：怎么样才算故障？CPU负载多少长算高

#####监控范围：
1. 硬件监控	服务器的硬件故障
2. 操作系统监控	CPU，内存，IO（硬盘和网络），进程等
3. 应用服务监控	Apache是否正常，DOWN机等
4. 业务监控	例如：今天下了多少单，今天客户客单价多少等

#####硬件监控
远程控制卡：和服务器没有太大的关系，服务器有没有操作系统也跟它无关系（DELL服务器：iDRAC，HP服务器：ILO，IBM服务器：IMM）

远程控制卡的标准是IPMI标准，有了IPMI标准，Linux就可以对IPMI标准进行监控，IPMI依赖于BMC控制器（BMC控制器放在远程控制卡上面），它们之间来沟通监控CPU温度，风扇转速，硬盘有没有报警

在Linux下用ipmitool工具来监控硬件

1. 硬件要支持（硬件要支持ipmi协议，并且不是虚拟机）
2. 操作系统要支持	Linux是支持IPMI的
3. 管理工具	ipmitool

使用IPMI有两种方式：

1. 本地调用
2. 远程调用（需要IP地址，用户名和密码）

IPMI配置网络有两种方式：

1. ipmi over lan  ipmi数据包通过服务器网卡来走
2. 独立网卡

硬件监控两种方式：

1. 使用IPMI
2. 机房巡检

路由器和交换机监控：使用SNMP（简单网络管理协议）监控，要开启SNMP协议

#####安装及使用ipmi
1. 安装ipmitool软件：yum install OpenIPMI ipmitool -y
2. 启动服务：systemctl start ipmi
3. ipmitool help ---获取ipmi的命令

Linux监控：

client端（安装net-snmp，需要启动代理服务）<<————>>server端（安装net-snmp-utils，不需要启动服务）

* 安装SNMP协议软件：yum install -y net-snmp net-snmp-utils
* 更改snmp配置文件：vim /etc/snmp/snmpd.conf，只添加如下一行即可

`rocommunity oldboy 192.168.1.201 `

----rocommunity是团体名（在zabbix中用$(rocommunity)表示）----oldboy是团体名的值，IP地址是要监控的主机，这里写的是本地主机，snmp需要snmp代理端起服务，不需要服务端起服务。然后使用工具来连接代理就可以获取数据。（相当于ssh，通过ssh命令就可以连接过去）

* systemctl start snmpd    ---开启snmp服务
* snmp默认监听的是TCP的199端口和UDP的161端口
* MIB对象的唯一标识符是OID（有两种表达方式，数字和字符串方式，数字例子：1.3.6.1.2.1.1.3.0）
* 获取系统启动时间：snmpget -v2c -c oldboy 192.168.1.201 1.3.6.1.2.1.1.3.0（-v2c为snmp的v2版本协议，-c oldboy为团体名，192.168.1.201 1.3.6.1.2.1.1.3.0获取本机IP的开机时间，【1.3.6.1.2.1.1.3.0这个为OID】）
* 获取系统负载：snmpget -v2c -c oldboy 192.168.1.201 1.3.6.1.4.1.2021.10.1.3.1（只能获取单独的1分钟负载）
* snmpwalk -v2c -c oldboy 192.168.1.201 1.3.6.1.4.1.2021.10.1.3(以树结点来获取到1分钟，5分钟，15分钟的负载)

监控的流程：
1. 收集
2. 存储
3. 展示
4. 报警

snmp有5种报文跟snmp代表来沟通

1. GetRequest PDU  （例如：snmpget -v2c -c oldboy 192.168.1.201 1.3.6.1.4.1.2021.10.1.3.1）
2. GetNextRequest PDU （例如： snmpwalk -v2c -c oldboy 192.168.1.201 1.3.6.1.4.1.2021.10.1.3）

###系统监控
1. CPU
2. 内存
3. IO input/output (网络，磁盘)

####CPU监控
CPU三个重要的概念

	一个标准的Linux可以运行50到5万个进程
	时间片
	1. 上下文切换:CPU调度器实施对进程的切换过程，称为上下文切换
	2. 运行队列（负载）：运行队列的多少来判别负载的大小
	3. 使用率：user time（用户态）,system time（系统态）
	
	确定服务类型：
		IO密集型：数据库
		CPU密集型：web mail

	确定性能基准线：
		运行队列：1-3个线程为正常，1CPU4核来计算，线程应不超过12为正常
		CPU使用：65%——70% 用户态利用率为正常
				30%-35% 内核态利用率为正常
				0%-5% 空闲为正常
		上下文切换：越少越好
#####TOP命令详解：
	CPU栏：
		us:用户态百分比使用率
		sy:内核态或系统态百分比使用率
		ni:nice值之间切换的百分比使用率
		id:CPU空闲百分比使用率
		wa:IO队列等待百分比使用率
		hi:CPU硬中断百分比使用率
		si:CPU软中断百分比使用率
		st:虚拟CPU等待实际CPU的百分比使用率
	内存栏：
		Mem total:全部物理内存
			free:空闲内存
			used:使用内存
			buff/cache:缓冲缓存内存，Linux尽量把不用的内存分存给buff/cache
		Swap内存和物理内存一样
		top动态窗口菜单：
			PID:进程ID
			user：进程所有者
			PR:优先级
			NI：nice值 
			VIRT:进程占用的虚拟内存
			RES：进程占用的物理内存
			SHR:共享内存
			S：进程状态
			%CPU：cpu使用率
			%MEM:内存使用率
			TIME+:进程启动后的运行时间
			COMMAND：命令
		快捷键：
			以内存排序：按大写的M进行排序
			以CPU排序：按大写的P进行排序
#####sysstat工具包
	vmstat工具-----监控cpu状态
		r:运行队列的状态
		b:进程阻塞，等待完成的状态
		in:中断
	mpstat工具:----监控
CPU详细的内容
		%nice：nice值改变时对CPU占用的百分比

####内存监控
linux是不知道物理内存和交换分区内存的

内存是分成页，硬盘是分面块

1. 寻址	2. 空间（连续的内存空间合并）
2. 共享内存是给进程与进程之前使用的，各使一点共享内存
3. vmstat 下  si：表示swap in 物理内存的，so:表示物理内存输到swap分区的，bi:块到内存的， bo：内存写到块的
4. 交换分区使用得越多是不行的。内存使用率在80%会报警

####硬盘监控 ---块
1. IOPS 	----IP's Per Second   每秒IO请求次数
2. IO分为：顺序IO和随机IO,顺序IO最块，最快有时候会接近内存的速度
3. yum install iotop -y   iotop工具
4. 监控硬盘用得最多的是iotop和iostat 

####网络监控
yum install -y iftop   iftop工具
iftop -n   查看网络流向

阿里测、奇云测、站长工具可测网站访问速度和dns解析情况等

TCP监控

IBM的nmon工具可生成性能报表

	使用方法：在linux下执行nmon二进制文件生成nmon报告文件，命令例如：./nmon16e_x86_rhel72 -s 10 -c 10 -f -m /tmp/  
	则会在/tmp下生成nmon报告文件，然后利用nmon analyser v55.xlsm这个文件来读取nmon文件生成excel形式的报告文件

####SecureCRT
SecureCRT下linuxt系统安装yum install lrzsz，即可使用rz -e命令上传，sz -e 命令下载 
 
####应用监控

#####例如：nginx------源码安装，尽量下载nginx的最新稳定版本

1. 安装nginx的依赖包：`yum install -y gcc glibc gcc-c++ pcre-devel openssl-devel`
2. `useradd -s /sbin/nologin -M www`  建立普通用户用于运行nginx
3. `./configure --prefix=/usr/local/nginx-1.14.0 --user=www --group=www --with-http_ssl_module --with-http_stub_status_module` 生成makefile文件（收集系统环境信息，用于编译用),设置程序安装路径，程序启动的用户和组，开启两个相关模块
4. make && make install  ----make是编译工作，make install把生成的文件复制到指定的目录下（生成的文件可以直接复制到同系统环境下运行）
5. `ln -s /usr/local/nginx-1.14.0/ /usr/local/nginx`生成软链接安装目录
6. `/usr/local/nginx/sbin/nginx -t`启动测试
7. `/usr/local/nginx/sbin/nginx`  启动服务 
8. `cd /usr/local/nginx/conf && vim nginx.conf`  修改nginx配置文件,只对192.168.1.0/24网段开启监控
	添加如下：

          `location /nginx-status {
             stub_status on;
             access_log off;
             allow 192.168.1.0/24;
             deny all;
          }`

9. 启动nginx服务：`/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf`
10. 重启nginx服务`/usr/local/nginx/sbin/nginx -s reload`
11. 停止nginx服务：ps -ef | grep nginx ; kill -9 pid

##安装Zabbix(CentOS7.5)
1. 安装zabbix yum源：
`rpm -ivh https://mirrors.aliyun.com/zabbix/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1.el7.noarch.rpm`
2. 安装相关软件：
`yum install zabbix-server zabbix-web zabbix-server-mysql zabbix-web-mysql mariadb-server mariadb zabbix-agent -y`
3. 修改PHP时区配置
`sed -i 's@# php_value date.timezone Europe/Riga@php_value date.timezone Asia/Shanghai@g' /etc/httpd/conf.d/zabbix.conf`
4. 启动mariadb数据库
`systemctl start mariadb`
5. 创建Zabbix所用的数据库及用户
	<pre>
	mysql
	create database zabbix character set utf8 collate utf8_bin;
	grant all on zabbix.* to zabbix@'localhost' identified by '123456';
	exit
	cd /usr/share/doc/zabbix-server-mysql-3.0.22/
	zcat create.sql.gz | mysql -uzabbix -p123456 zabbix
	</pre>
6. 修改zabbix配置
	<pre>
	#vim /etc/zabbix/zabbix_server.conf
	DBHost=localhost	#数据库所在主机
	DBName=zabbix		#数据库名
	DBUser=zabbix		#数据库用户
	DBPassword=123456	#数据库密码
	</pre>
7. 启动Zabbix及http
`systemctl start zabbix-server  --如果启动失败，使用yum update  更新系统内核`  
`systemctl start httpd`
8. WEB上配置zabbix
	<pre>
	1.输入web上配置zabbx-srver的地址：http://zabbix-IP/zabbix/setup.php,进入配置
	2.填写数据库地址，端口，用户，密码，及zabbix-server在web上右上角展示的名称，直至配置完成
	</pre>
9. 输入zabbix-server管理地址进行管理配置：http://zabbix-IP/zabbix
10. zabbix默认用户名为：Admin 密码为：zabbix,登进去后第一步更改密码
11. 配置agent端：
	<pre>
	#vim /etc/zabbix/zabbix_agentd.conf
	Server=127.0.0.1	#设置被动端的zabbix-server地址，等待客户端汇报
	ServerActive=127.0.0.1	#设置主动端的zabbix-server地址，服务端主动抓取
	systemctl start zabbix-agent.service  --启动zabbix-agent
	</pre>
12. netstat -tunlp --查看zabbix-server zabbix-agent httpd 服务是否正常启动

###添加Zabbix自定义监控项
####拿nginx来监控
1. vim /etc/zabbix/zabbix_agentd.d.conf  可查看到include=/etc/zabbix/zabbix_agentd.d,此目录下所有配置将备引用，所以在/etc/zabbix/zabbix_agentd.d目录下新建一个nginx应用监控的配置文件nginx.active,用来当作nginx的监控项
2. 编辑/etc/zabbix/zabbix_agentd.d/nginx.conf文件
 <pre>vim /etc/zabbix/zabbix_agentd.d/nginx.conf
UserParameter=nginx.active,/usr/bin/curl -s http://192.168.1.201:8080/nginx-status |grep 'Active' | awk '{print $NF}' </pre>
3. systemctl restart zabbix-agent
4. yum install zabbix-get -y 
5. vim /etc/zabbix/zabbix-agentd.conf  #把Server=127.0.0.1设置成192.168.1.201，这样下一步才不会报错
6. zabbix_get -s 192.168.1.201 -p 10050 -k "nginx.active"  #测试获取值是否设置成功
7. 在zabbix-web界面上创建item监控项。



