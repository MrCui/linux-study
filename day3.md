#### 1.必知必会文件

```shell
/etc/sysconfig/network-scripts/ifcfg-eth0
# eth0网卡的配置文件
```
```shell
/etc/resolv.conf
# 该文件是DNS域名解析的配置文件

# nameserver   定义DNS服务器的IP地址
# domain       定义本地域名
# search       定义域名的搜索列表
# sortlist     对返回的域名进行排序
# /etc/resolv.conf的一个示例：

domain test.com
search www.test.com test.com
nameserver 202.96.128.86
nameserver 202.96.128.166
# 最主要是nameserver关键字，如果没指定nameserver就找不到DNS服务器，其它关键字是可选的。
```
```shell
/etc/sysconfig/network

# 改文件主要用于配置网络信息, 主机名, 网关.
# NETWORKING=yes 
# FORWARD_IPV4=yes 
# HOSTNAME=oldboy
# GATEWAY= 
```
```shell
/etc/hosts
# 本机的域名解析文件
# 格式
127.0.0.1 oldboy.com
```
```shell
/etc/fstab
# 磁盘被手动挂载之后都必须把挂载信息写入/etc/fstab这个文件中，否则下次开机启动时仍然需要重新挂载。系统开机时会主动读取/etc/fstab这个文件中的内容，根据文件里面的配置挂载磁盘。这样我们只需要将磁盘的挂载, 信息写入这个文件中我们就不需要每次开机启动之后手动进行挂载了。
#
# /etc/fstab
# Created by anaconda on Thu Aug 14 21:16:42 2014
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=94e4e384-0ace-437f-bc96-057dd64f42ee / ext4 defaults,barrier=0 1 1
tmpfs                   /dev/shm                tmpfs   defaults        0 0
devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
sysfs                   /sys                    sysfs   defaults        0 0
proc                    /proc                   proc    defaults        0 0
# 第一列：Device：磁盘设备文件或者该设备的Label或者UUID
# 第二列：Mount point：设备的挂载点，就是你要挂载到哪个目录下。
# 第三列：filesystem：磁盘文件系统的格式，包括ext2、ext3、reiserfs、nfs、vfat等
# 第四列：parameters：文件系统的参数
# 第五列：能否被dump备份命令作用：dump是一个用来作为备份的命令。通常这个参数的值为0或者1
# 第六列: 第六列：是否检验扇区：开机的过程中，系统默认会以fsck检验我们系统是否为完整（clean）。
```
```shell
/etc/inittab
# 
# /etc/inittab文件的作用
# October 9th, 2010 | Categories: OS | Tags: inittab, Linux
# http://www.dbconf.net/etc-inittab-file-role.html
# Linux启动之后运行的第一个进程init会从/etc/inittab 文件中读取配置并做一些初始化的工作# 

# 格式：

# id:runlevels:action:process# 

# 大致的含义就是init初始化程序以一定的方式(action)在哪些运行级别(runlevels)下执行某个程序(process)# 

# id

# 用 于表示该配置项的id，长度为1~4个字符，id必须在本文件内唯一，如果有重复id，则init启动的时候会报duplicate id的错误，且后面的重复项配置将无效。这边可以将每一条配置项看作数据库中的一条记录，分号分割的部分为各个字段，将id号则可以看作记录的 Primary Key。# 

# runlevels

# 程序执行的运行级别。

# action

# 常见的action如下所示：

# * initdefault: 该配置项指定系统的默认运行级。
# * respawn: 该配置项所指定的进程如果被结束，则重新启动该进程。
# * wait: 该配置项指定的进程在运行结束之前不要执行下一条配置项。
# * once: 切换到对应的运行级之后，仅执行指定的进程一次。
# * sysinit: 无论以什么运行级启动，系统启动时都要执行该配置项指定的进程。
# * boot: 仅在系统启动时执行一次。
# * bootwait: 仅在系统启动时执行一次，在执行结束之前不执行下一条配置项
# * powerfail: 当接收到UPS的断电通知时执行该项指定的进程。
# * powerwait: 与powerfail相同，但init会等待进程执行结束。
# * powerokwait: 接收到UPS的供电通知时执行。
# * ctrlaltdel: 当用户同时按下 Ctrl+Alt+Del 时执行该项指定的进程。# 

# process:

# 指定的程序路径和参数

# 例子：

# 举一个最常见的例子，就是默认地我们可以使用Ctrl+Alt+F1~F6切换的虚拟终端功能就是在此文件中定义的：# 

# 1:2345:respawn:/sbin/mingetty tty1
# 2:2345:respawn:/sbin/mingetty tty2
# 3:2345:respawn:/sbin/mingetty tty3
# 4:2345:respawn:/sbin/mingetty tty4
# 5:2345:respawn:/sbin/mingetty tty5
# 6:2345:respawn:/sbin/mingetty tty6# 

# id列可以用数字，也可以用字符，当然数字也是当字符来看，只要确保本文件唯一即可。

# runlevel定义了在2~5的运行级别都存在对应的虚拟终端，所以在2~5的运行级别下可以通过Ctrl+Alt+F1~F6进行终端的切换，而在s[单用户模式]和运行级别为1的时候则无法使用虚拟终端[虽然可以通过Ctrl+Alt+F1~F6进行切换，但终端已经被挂起]。

# respawn则定义了如果mingetty进程结束，则重启该进程。

# /sbin/mingetty ttyn定义了执行的程序和参数。

# 要想/etc/inittab即时生效，可以通过以下命令让init进程重新读取此文件:# 

# init q
```
```shell
/etc/rc.local
# 该脚本是在系统初始化级别脚本运行之后再执行的，因此可以安全地在里面添加你想在系统启动之后执行的脚本。
```
```shell
/etc/profile
# 此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行。
```
```shell
/etc/bashrc
# 为每一个运行bash shell的用户执行此文件。
```
```shell
/usr/local
# 第三方软件编译安装的默认目录
```
```shell
/var/log/messages
# 系统的日志信息
```
```shell
/var/log/secure
# 包含验证和授权方面的日志信息。例如，sshd会将所有信息记录（其中包括失败登录）在这里。
```
```shell
/var/spool/cron/root
# root用户的crontab信息记录在这里. 查看方法 crontab -uroot -l 或者直接vim这个文件
```
```shell
/proc/cpuinfo
# CPU状态信息
```
```shell
/proc/meminfo
# 内存状态信息
```
```shell
/proc/loadavg
# 负载状态信息
```
```shell
/proc/mounts
# 挂载信息
```


##### 2.如何修改DNS

 ```shell
/etc/resolv.conf       
/etc/sysconfig/network-scripts/ifcfg-eth0

# 主要是修改网卡的配置 如ifcfg-eth0 里的DNS1 DNS2
 ```



##### 3.如何修改主机名

```shell
/etc/sysconfig/network
# 修改其中的 HOSTNAME
```



##### 4.linux下面安装软件方法

```shell
$ yum 安装
$ rpm 安装
$ 编译安装
```



##### 5.rpm

 ```shell
# rpm命令是RPM软件包的管理工具。
# 语法:
$ rpm(选项)(参数)

# 选项:
# -a：查询所有套件；
# -b<完成阶段><套件档>+或-t <完成阶段><套件档>+：设置包装套件的完成阶段，并指定套件档的文件名称；
# -c：只列出组态配置文件，本参数需配合"-l"参数使用；
# -d：只列出文本文件，本参数需配合"-l"参数使用；
# -e<套件档>或--erase<套件档>：删除指定的套件；
# -f<文件>+：查询拥有指定文件的套件；
# -h或--hash：套件安装时列出标记；
# -i：显示套件的相关信息；
# -i<套件档>或--install<套件档>：安装指定的套件档；
# -l：显示套件的文件列表；
# -p<套件档>+：查询指定的RPM套件档；
# -q：使用询问模式，当遇到任何问题时，rpm指令会先询问用户；
# -R：显示套件的关联性信息；
# -s：显示文件状态，本参数需配合"-l"参数使用；
# -U<套件档>或--upgrade<套件档>：升级指定的套件档；
# -v：显示指令执行过程；

# 参数:
# 软件包

 ```



##### 6.练习

#######1.如何过滤出已知当前目录下 oldboy中的所有一级目录(提示:不包含 oldboy目录下面目录的子目录及隐藏目录，即只能是一级目录)？

```shell
$ find  -maxdepth 1   -type d  ! -name "."
```

######2.假如当前目录是如下命令的结果：

```shell
[root@oldboy oldboy] pwd #==>这是打印当前目录的，最菜的命令了，你该会的。
/oldboy
```

######现在因为需要进入到了/tmp 目录下进行操作，执行的命令如下：

```shell
[root@oldboy oldboy] cd /tmp
[root@oldboy tmp] pwd
/tmp
```

######操作完毕后，希望快速返回上一次进入的目录，即/oldboy 目录，该如何做呢？（提示：不

######能用 cd /oldboy 命令呦)

```shell
$ cd -
```

######3.一个目录中有很多文件（ls -l 查看时好多屏），想用一条命令最快速度查看到最近更新的文件。如何看？

```shell
$ ll -tr
```

######4.在配置 apache时 执行了./configure --prefix=/application/apache2.2.17 来编译 apche，在 make install 完成后，希望用户访问 apache 路径更简单，需要给/application/apache2.2.17 目录做一个软链接/application/apache，使得内部开发或管理人员通过/application/apache 就可以访问到apache 的安装目录/application/apache2.2.17 下的内容，请你给出实现的命令。（提示： apache为一个 web 服务）

```shell
$ ln -s /appliaction/apache2.2.17/ /appliaction/apache
```

######5.已知 apache 服务的访问日志按天记录在服务器本地目录/app/logs 下，由于磁盘空间紧张，现在要求只能保留最近 7 天的访问日志！请问如何解决？ 请给出解决办法或配置或处理命令。（提示：可以从 apache 服务配置上着手，也可以从生成出来的日志上着手。）

```shell
$ rm $(find /app/logs/ -type f -name "*log"  -mtime +7 ) -rf
```

######6.调试系统服务时，希望能实时查看/var/log/messages 系统日志的更新,如何做？

```shell
$ tail -f /var/log/messages
```

######7.打印轻量级 web 服务的配置文件 nginx.conf 内容的行号及内容，该如何做？

```shell
$ cat -n nginx.conf
```

######8.装完 Centos 系统后，希望网络文件共享服务 NFS，仅在 3 级别上开机自启动，该如何做？

```shell
$ chkconfig --level 012456 nfs off;
$ chkconfig --level 3 nfs on;
```

######9.linux 系统运行级别一般为 0-6，请分别写出每个级别的含义。

```shell
# 0 停机 
# 1 单用户模式 
# 2 多用户，没有 NFS 
# 3 完全多用户模式 
# 4 没有用到 
# 5 图形界面 
# 6 重新启动
```

######10.linux 系统中查看中文乱码，请问如何解决乱码问题？

```shell
# 更改ssh工具字符集为utf-8 更改系统字符集为utf-8
```

###### 11.如何优化 linux 系统（可以不说太具体）？

```shell
# 关闭 selinx 关闭 iptables
```



##### 7.命令总结(例子)

```shell
# df
# 用于显示磁盘分区上的可使用的磁盘空间。默认显示单位为KB。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。
# 语法
$ df(选项)(参数)

# 选项
-a或--all：包含全部的文件系统；
--block-size=<区块大小>：以指定的区块大小来显示区块数目；
-h或--human-readable：以可读性较高的方式来显示信息；
-H或--si：与-h参数相同，但在计算时是以1000 Bytes为换算单位而非1024 Bytes；
-i或--inodes：显示inode的信息；
-k或--kilobytes：指定区块大小为1024字节；
-l或--local：仅显示本地端的文件系统；
-m或--megabytes：指定区块大小为1048576字节；
--no-sync：在取得磁盘使用信息前，不要执行sync指令，此为预设值；
-P或--portability：使用POSIX的输出格式；
--sync：在取得磁盘使用信息前，先执行sync指令；
-t<文件系统类型>或--type=<文件系统类型>：仅显示指定文件系统类型的磁盘信息；
-T或--print-type：显示文件系统的类型；
-x<文件系统类型>或--exclude-type=<文件系统类型>：不要显示指定文件系统类型的磁盘信息；
--help：显示帮助；
--version：显示版本信息。

# 参数
# 文件

```
```shell
# free
# 可以显示当前系统未使用的和已使用的内存数目，还可以显示被内核使用的内存缓冲区。
# 语法
$ free(选项)

# 选项
-b：以Byte为单位显示内存使用情况；
-k：以KB为单位显示内存使用情况；
-m：以MB为单位显示内存使用情况；
-o：不显示缓冲区调节列；
-s<间隔秒数>：持续观察内存使用状况；
-t：显示内存总和列；
-V：显示版本信息。
```
```shell
# w 
# 用于显示已经登陆系统的用户列表，并显示用户正在执行的指令。执行这个命令可得知目前登入系统的用户有那些人，以及他们正在执行的程序。单独执行w命令会显示所有的用户，您也可指定用户名称，仅显示某位用户的相关信息。 # 语法 
$ w(选项)(参数)

# 选项
-h：不打印头信息；
-u：当显示当前进程和cpu时间时忽略用户名；
-s：使用短输出格式；
-f：显示用户从哪登录；
-V：显示版本信息。

# 参数
用户：仅显示指定用户。

```
```shell
# init
# 是Linux下的进程初始化工具，init进程是所有Linux进程的父进程，它的进程号为1。init命令是Linux操作系统中不可缺少的程序之一，init进程是Linux内核引导运行的，是系统中的第一个进程。
# 语法
$ init(选项)(参数)

# 选项
-b：不执行相关脚本而直接进入单用户模式；
-s：切换到单用户模式。

# 参数
# 运行等级：指定Linux系统要切换到的运行等级。

```
```shell
# runlevel
# 用于打印当前Linux系统的运行等级。
# 语法
$ runlevel

# 运行级别
# 0 停机 1 单用户模式 2 多用户，没有 NFS 3 完全多用户模式 4 没有用到 5 图形界面 6 重新启动
```
```shell
# tail
# 用于输入文件中的尾部内容。tail命令默认在屏幕上显示指定文件的末尾10行。如果给定的文件不止一个，则在显示的每个文件前面加一个文件名标题。如果没有指定文件或者文件名为“-”，则读取标准输入。
# 语法
$ tail(选项)(参数)

# 选项
--retry：即是在tail命令启动时，文件不可访问或者文件稍后变得不可访问，都始终尝试打开文件。使用此选项时需要与选项“——follow=name”连用；
-c<N>或——bytes=<N>：输出文件尾部的N（N为整数）个字节内容；
-f<name/descriptor>或；--follow<nameldescript>：显示文件最新追加的内容。“name”表示以文件名的方式监视文件的变化。“-f”与“-fdescriptor”等效；
-F：与选项“-follow=name”和“--retry"连用时功能相同；
-n<N>或——line=<N>：输出文件的尾部N（N位数字）行内容。
--pid=<进程号>：与“-f”选项连用，当指定的进程号的进程终止后，自动退出tail命令；
-q或——quiet或——silent：当有多个文件参数时，不输出各个文件名；
-s<秒数>或——sleep-interal=<秒数>：与“-f”选项连用，指定监视文件变化时间隔的秒数；
-v或——verbose：当有多个文件参数时，总是输出各个文件名；
--help：显示指令的帮助信息；
--version：显示指令的版本信息。

# 参数
# 文件列表：指定要显示尾部内容的文件列表。
```
```shell
# du
# 也是查看使用空间的，但是与df命令不同的是Linux du命令是对文件和目录磁盘使用的空间的查看，还是和df命令有一些区别的。
# 语法
$ du [选项][文件]

# 选项
-a或-all 显示目录中个别文件的大小。
-b或-bytes 显示目录或文件大小时，以byte为单位。
-c或--total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
-k或--kilobytes 以KB(1024bytes)为单位输出。
-m或--megabytes 以MB为单位输出。
-s或--summarize 仅显示总计，只列出最后加总的值。
-h或--human-readable 以K，M，G为单位，提高信息的可读性。
-x或--one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
-L<符号链接>或--dereference<符号链接> 显示选项中所指定符号链接的源文件大小。
-S或--separate-dirs 显示个别目录的大小时，并不含其子目录的大小。
-X<文件>或--exclude-from=<文件> 在<文件>指定目录或文件。
--exclude=<目录或文件> 略过指定的目录或文件。
-D或--dereference-args 显示指定符号链接的源文件大小。
-H或--si 与-h参数相同，但是K，M，G是以1000为换算单位。
-l或--count-links 重复计算硬件链接的文件。
```
```shell
# history
# 用于显示指定数目的指令命令，读取历史命令文件中的目录到历史命令缓冲区和将历史命令缓冲区中的目录写入命令文件。
# 语法
$ history(选项)(参数)

# 选项
-c：清空当前历史命令；
-a：将历史命令缓冲区中命令写入历史命令文件中；
-r：将历史命令文件中的命令读入当前历史命令缓冲区；
-w：将当前历史命令缓冲区命令写入历史命令文件中。

# 参数
# n：打印最近的n条历史命令。
```





##### 1.打包压缩

```shell
# tar
-c: 建立压缩档案
-x：解压
-t：查看内容
-r：向压缩归档文件末尾追加文件
-u：更新原压缩包中的文件

# 这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需要在压缩或解压档案时可选的。

-z：有gzip属性的
-j：有bz2属性的
-Z：有compress属性的
-v：显示所有过程
-O：将文件解开到标准输出

# 下面的参数-f是必须的

-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

$ tar -cf all.tar *.jpg
# 这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包，-f指定包的文件名。

$ tar -rf all.tar *.gif
# 这条命令是将所有.gif的文件增加到all.tar的包里面去。-r是表示增加文件的意思。

$ tar -uf all.tar logo.gif
# 这条命令是更新原来tar包all.tar中logo.gif文件，-u是表示更新文件的意思。

$ tar -tf all.tar
# 这条命令是列出all.tar包中所有文件，-t是列出文件的意思

$ tar -xf all.tar
# 这条命令是解出all.tar包中所有文件，-t是解开的意思

# 压缩
$ tar -cvf jpg.tar *.jpg # 将目录里所有jpg文件打包成tar.jpg 
$ tar -czf jpg.tar.gz *.jpg   # 将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
$ tar -cjf jpg.tar.bz2 *.jpg # 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
$ tar -cZf jpg.tar.Z *.jpg   # 将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
$ rar a jpg.rar *.jpg # rar格式的压缩，需要先下载rar for linux
$ zip jpg.zip *.jpg # zip格式的压缩，需要先下载zip for linux

# 解压

$ tar -xvf file.tar # 解压 tar包
$ tar -xzvf file.tar.gz # 解压tar.gz
$ tar -xjvf file.tar.bz2   # 解压 tar.bz2
$ tar -xZvf file.tar.Z   # 解压tar.Z
$ unrar e file.rar # 解压rar
$ unzip file.zip # 解压zip

# 总结

1、*.tar 用 tar -xvf 解压
2、*.gz 用 gzip -d或者gunzip 解压
3、*.tar.gz和*.tgz 用 tar -xzf 解压
4、*.bz2 用 bzip2 -d或者用bunzip2 解压
5、*.tar.bz2用tar -xjf 解压
6、*.Z 用 uncompress 解压
7、*.tar.Z 用tar -xZf 解压
8、*.rar 用 unrar e解压
9、*.zip 用 unzip 解压
```



##### 2.linux文件属性

**类型**

> | -    | 普通文件     |
> | ---- | -------- |
> | d    | 目录文件     |
> | l    | 链接文件     |
> | b    | 块设备文件    |
> | c    | 字符型设备文件  |
> | s    | socket文件 |
> | p    | 管道类型文件   |

**权限** 

| 对应数字 | 权限   |      |
| ---- | ---- | ---- |
| r    | 4    | 读    |
| w    | 2    | 写    |
| x    | 1    | 执行   |