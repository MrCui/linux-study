#### 1. find命令

##### a) sed替换

```shell
$ sed -i 's#../idctest_iplist#test2#g' `find ./ -type f -name "*.sh"`
```



##### b) rm删除 

```shell
$ find ./ -type f -name "2*.txt" -exec rm -i {} \;
# -exec  参数后面跟的是command命令，它的终止是以;为结束标志的，所以这句命令后面的分号是不可缺少的，考虑到各个系统中分号会有不同的意义，所以前面加反斜杠(转义)。
```



##### c) cat显示内容

```shell
$ find ./ -type f -name "*.sh" | xargs cat
# xargs的作用, 管道实现的是将前面的stdout作为后面的stdin，但是有些命令不接受管道的传递方式，最常见的就是ls命令。有些时候命令希望管道传递的是参数，但是直接用管道有时无法传递到命令的参数位，这时候需要xargs，xargs实现的是将管道传输过来的stdin进行处理然后传递到命令的参数位上。也就是说xargs完成了两个行为：处理管道传输过来的stdin；将处理后的传递到正确的位置上。
```



##### d) cp复制的方法 1

```shell
$ cp `find ./ -type f -name "*.sh"` ./test3/
# 执行命令$(command)  结果不输出直接返回;  上面的写法等效
```



#####e) cp复制的方法 2

```shell
$ find ./ -type f -name "*.sh"|xargs cp -t ./test3/
# cp -t 参数会将cp命令的 目标和源参数的顺序颠倒. 因为xargs默认将经由管道的标准输出转化为标准输入 放置命令的最后面
```



##### f)  cp复制的方法 3

```shell
$ find ./ -type f -name "*.sh"|xargs -i cp {} ./test3/
# 原理同上, 但不在利用cp命令本身的参数, 而是利用 xargs命令的参数 -i 将标准输入放置{}的位置
```



##### g)  cp复制的方法 4

```shell
$ find ./test2 -type f -name "*.sh" -exec cp {} ./test3 \;
# 利用find的命令的exec执行cp命令.  注意这样执行跟上述方法都不一样, 上述方法都是得到所有查询结果再执行, 该方法是得到一个结果执行一次.
```



#### 2. 关闭selinux, iptables

#####a) selinux

```shell
# 查看状态的command
getenforce
# 或
/usr/sbin/sestatus -v  
# enforcing selinux开启了 正在运行
# permissive 临时关闭了 会有一些警告信息
# disablied  selinux彻底关闭 


# 永久关闭-修改配置文件
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#       enforcing - SELinux security policy is enforced.
#       permissive - SELinux prints warnings instead of enforcing.
#       disabled - SELinux is fully disabled.
SELINUX=disabled
# SELINUXTYPE= type of policy in use. Possible values are:
#       targeted - Only targeted network daemons are protected.
#       strict - Full SELinux protection.
SELINUXTYPE=targeted

#重启
```



#####b) iptables

```shell
# 查看状态 注意只有root用户允许操作iptables
$ sudo service iptables status

# 临时关闭
$ sudo service iptables stop

# 永久关闭
chkconfig iptables off

# linux下的服务/软件 临时关闭基本都是service software-name stop
# 永久关闭基本都是chkconfig software-name off 
```



#### 3. linux下面显示中文乱码 排查及解决

```shell
# 检查linux系统的字符集
$ vim /etc/sysconfig/i18n 

LANG="en_US.UTF-8"
SYSFONT="latarcyrheb-sun16"

# 检查远程连接工具的字符集, 二者匹配即可. 基本现在都是使用utf-8编码

```



#### 4.linux目录结构特点

```shell
# linux下一切皆文件, linux下一切目录均已 根目录(/) 开始

# linux下面的目录可以随便挂载到不同的磁盘分区上
tree -L 1
.
├── bin
├── boot
├── data
├── dev
├── etc
├── home
├── lib
├── lib64
├── lost+found
├── media
├── mnt
├── opt
├── proc
├── root
├── sbin
├── selinux
├── srv
├── sys
├── tmp
├── usr
├── var
└── www

# /bin：
# bin是Binary的缩写, 这个目录存放着最经常使用的命令。
# /boot：
# 这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。
# /dev ：
# dev是Device(设备)的缩写, 该目录下存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。
# /etc：
# 这个目录用来存放所有的系统管理所需要的配置文件和子目录。
# /home：
# 用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。
# /lib：
# 这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。
# /lost+found：
# 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。
# /media：
# linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。
# /mnt：
# 系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。
# /opt：
#  这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。
# /proc：
# 这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
# 这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：
# echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
# /root：
# 该目录为系统管理员，也称作超级权限者的用户主目录。
# /sbin：
# s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
# /selinux：
#  这个目录是Redhat/CentOS所特有的目录，Selinux是一个安全机制，类似于windows的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。
# /srv：
#  该目录存放一些服务启动之后需要提取的数据。
# /sys：
#  这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 。
# sysfs文件系统集成了下面3种文件系统的信息：针对进程信息的proc文件系统、针对设备的devfs文件系统以及针对伪终端的devpts文件系统。
# 该文件系统是内核设备树的一个直观反映。
# 当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。
# /tmp：
# 这个目录是用来存放一些临时文件的。
# /usr：
#  这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与windows下的program files目录。
# /usr/bin：
# 系统用户使用的应用程序。
# /usr/sbin：
# 超级用户使用的比较高级的管理程序和系统守护程序。
# /usr/src：内核源代码默认的放置目录。
# /var：
# 这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。


```



#### 5.linux网卡的配置文件

```shell
$ vim /etc/sysconfig/network-scripts/ifcfg-eth0 # 修改eth0网卡

DEVICE=eth0                # 网卡名字 第一块网卡 eth0   
HWADDR=00:0c:29:01:55:7a   # HWADDR hardware address 硬件地址/MAC地址
TYPE=Ethernet              # 网络类型 
UUID=					   #系统中唯一的号码  设备号
ONBOOT=yes                 # ****** boot on 在开机或重启网卡的时候  这块网卡是否会自动启动
                           # 默认是no
NM_CONTROLLED=yes          # 是否通过网络服务管理
BOOTPROTO=static           # ****** 网卡获取ip地址的方式 
                           # none/static 固定的 人工设置的 固定ip 
                           # dhcp(默认)   自动获取ip地址 
IPADDR=10.0.0.200          # ip地址
NETMASK=255.255.255.0      # 子网掩码
GATEWAY=10.0.0.2           # 网关
DNS1=					   # 主DNS地址
DNS2=					   # 备DNS地址


```



#### 6. DNS是什么

```shell
#DNS (Domain Name System，域名系统), 因特网上作为域名和IP地址相互映射的一个分布式数据库，能够使用户更方便的访问互联网，而不用去记住能够被机器直接读取的IP地址. 通过域名，最终得到该域名对应的IP地址的过程叫做域名解析（或主机名解析）。DNS协议运行在UDP协议之上，使用端口号53。

# 修改DNS 更改网卡的配置
vim /etc/sysconfig/network-scripts/ifcfg-eth0

DNS1=					   # 主DNS地址
DNS2=					   # 备DNS地址
```



#### 7. 排查linux连接不上网络

```shell
# 1. 检查网络是否通畅, ping 外网ip
# 2. 检查DNS是否正常, ping 域名
```



#### 8. 命令总结

##### a) find命令

```shell
# 语法
find [选项] [参数]

# 参数
-amin<分钟>：查找在指定时间曾被存取过的文件或目录，单位以分钟计算；
-anewer<参考文件或目录>：查找其存取时间较指定文件或目录的存取时间更接近现在的文件或目录；
-atime<24小时数>：查找在指定时间曾被存取过的文件或目录，单位以24小时计算；
-cmin<分钟>：查找在指定时间之时被更改过的文件或目录；
-cnewer<参考文件或目录>查找其更改时间较指定文件或目录的更改时间更接近现在的文件或目录；
-ctime<24小时数>：查找在指定时间之时被更改的文件或目录，单位以24小时计算；
-daystart：从本日开始计算时间；
-depth：从指定目录下最深层的子目录开始查找；
-expty：寻找文件大小为0 Byte的文件，或目录下没有任何子目录或文件的空目录；
-exec<执行指令>：假设find指令的回传值为True，就执行该指令；
-false：将find指令的回传值皆设为False；
-fls<列表文件>：此参数的效果和指定“-ls”参数类似，但会把结果保存为指定的列表文件；
-follow：排除符号连接；
-fprint<列表文件>：此参数的效果和指定“-print”参数类似，但会把结果保存成指定的列表文件；
-fprint0<列表文件>：此参数的效果和指定“-print0”参数类似，但会把结果保存成指定的列表文件；
-fprintf<列表文件><输出格式>：此参数的效果和指定“-printf”参数类似，但会把结果保存成指定的列表文件；
-fstype<文件系统类型>：只寻找该文件系统类型下的文件或目录；
-gid<群组识别码>：查找符合指定之群组识别码的文件或目录；
-group<群组名称>：查找符合指定之群组名称的文件或目录；
-help或——help：在线帮助；
-ilname<范本样式>：此参数的效果和指定“-lname”参数类似，但忽略字符大小写的差别；
-iname<范本样式>：此参数的效果和指定“-name”参数类似，但忽略字符大小写的差别；
-inum<inode编号>：查找符合指定的inode编号的文件或目录；
-ipath<范本样式>：此参数的效果和指定“-path”参数类似，但忽略字符大小写的差别；
-iregex<范本样式>：此参数的效果和指定“-regexe”参数类似，但忽略字符大小写的差别；
-links<连接数目>：查找符合指定的硬连接数目的文件或目录；
-iname<范本样式>：指定字符串作为寻找符号连接的范本样式；
-ls：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出；
-maxdepth<目录层级>：设置最大目录层级；
-mindepth<目录层级>：设置最小目录层级；
-mmin<分钟>：查找在指定时间曾被更改过的文件或目录，单位以分钟计算；
-mount：此参数的效果和指定“-xdev”相同；
-mtime<24小时数>：查找在指定时间曾被更改过的文件或目录，单位以24小时计算；
-name<范本样式>：指定字符串作为寻找文件或目录的范本样式；
-newer<参考文件或目录>：查找其更改时间较指定文件或目录的更改时间更接近现在的文件或目录；
-nogroup：找出不属于本地主机群组识别码的文件或目录；
-noleaf：不去考虑目录至少需拥有两个硬连接存在；
-nouser：找出不属于本地主机用户识别码的文件或目录；
-ok<执行指令>：此参数的效果和指定“-exec”类似，但在执行指令之前会先询问用户，若回答“y”或“Y”，则放弃执行命令；
-path<范本样式>：指定字符串作为寻找目录的范本样式；
-perm<权限数值>：查找符合指定的权限数值的文件或目录；
-print：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式为每列一个名称，每个名称前皆有“./”字符串；
-print0：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式为全部的名称皆在同一行；
-printf<输出格式>：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式可以自行指定；
-prune：不寻找字符串作为寻找文件或目录的范本样式;
-regex<范本样式>：指定字符串作为寻找文件或目录的范本样式；
-size<文件大小>：查找符合指定的文件大小的文件；
-true：将find指令的回传值皆设为True；
-typ<文件类型>：只寻找符合指定的文件类型的文件；
-uid<用户识别码>：查找符合指定的用户识别码的文件或目录；
-used<日数>：查找文件或目录被更改之后在指定时间曾被存取过的文件或目录，单位以日计算；
-user<拥有者名称>：查找符和指定的拥有者名称的文件或目录；
-version或——version：显示版本信息；
-xdev：将范围局限在先行的文件系统中；
-xtype<文件类型>：此参数的效果和指定“-type”参数类似，差别在于它针对符号连接检查。

# 例子
$ find ./test2 -type f -name "*.sh" # 查找当前目录中test2目录下的 所有以.sh结尾的文件
```



#####b) xargs命令

```shell
# xargs命令是给其他命令传递参数的一个过滤器，也是组合多个命令的一个工具。它擅长将标准输入数据转换成命令行参数，xargs能够处理管道或者stdin并将其转换成特定命令的命令参数。xargs也可以将单行或多行文本输入转换为其他格式，例如多行变单行，单行变多行。xargs的默认命令是echo，空格是默认定界符。这意味着通过管道传递给xargs的输入将会包含换行和空白，不过通过xargs的处理，换行和空白将被空格取代。xargs是构建单行命令的重要组件之一。

# 参数
-n 多行输出
-d 可以自定义一个定界符
-i 使用-i指定一个替换字符串{}，这个字符串({})在xargs执行时会被替换掉

# 例子
find ./ -type f -name "*.sh"|xargs cp -t ./test3/ # 查找当前目录下所有以.sh结尾的文件并用cp命令copy所有查询到的文件到test3目录下
```



##### e) df命令

```shell
# 语法
df [选项] [参数]

# 参数
-a 或--all：包含全部的文件系统；
--block-size=<区块大小>：以指定的区块大小来显示区块数目；
-h 或--human-readable：以可读性较高的方式来显示信息；
-H 或--si：与-h参数相同，但在计算时是以1000 Bytes为换算单位而非1024 Bytes；
-i 或--inodes：显示inode的信息；
-k 或--kilobytes：指定区块大小为1024字节；
-l 或--local：仅显示本地端的文件系统；
-m 或--megabytes：指定区块大小为1048576字节；
--no-sync：在取得磁盘使用信息前，不要执行sync指令，此为预设值；
-P 或--portability：使用POSIX的输出格式；
--sync：在取得磁盘使用信息前，先执行sync指令；
-t <文件系统类型>或--type=<文件系统类型>：仅显示指定文件系统类型的磁盘信息；
-T 或--print-type：显示文件系统的类型；
-x <文件系统类型>或--exclude-type=<文件系统类型>：不要显示指定文件系统类型的磁盘信息；
--help：显示帮助；
--version：显示版本信息。
# 例子
$ df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/xvda1             40G  7.4G   31G  20% /
tmpfs                 938M     0  938M   0% /dev/shm
/dev/xvdb1             50G  8.0G   39G  18% /mnt/data1
```



##### f) yum命令

```shell
# 语法
yum [选项] [参数]

# 选项
-h：显示帮助信息；
-y：对所有的提问都回答“yes”；
-c：指定配置文件；
-q：安静模式；
-v：详细模式；
-d：设置调试等级（0-10）；
-e：设置错误等级（0-10）；
-R：设置yum处理一个命令的最大等待时间；
-C：完全从缓存中运行，而不去下载或者更新任何头文件。

# 参数
install：安装rpm软件包；
update：更新rpm软件包；
check-update：检查是否有可用的更新rpm软件包；
remove：删除指定的rpm软件包；
list：显示软件包的信息；
search：检查软件包的信息；
info：显示指定的rpm软件包的描述信息和概要信息；
clean：清理yum过期的缓存；
shell：进入yum的shell提示符；
resolvedep：显示rpm软件包的依赖关系；
localinstall：安装本地的rpm软件包；
localupdate：显示本地rpm软件包进行更新；
deplist：显示rpm软件包的所有依赖关系。

# 例子
$ yum isntall tree
```



