#####1.awk指定多个分隔符-第二关题目
```shell
# -F fs   fs指定输入分隔符，fs可以是字符串或正则表达式，如-F:
$ awk -F"[,']" # 以逗号和分号为分隔符
```
#####2.打包压缩的方法 tar  zip 
```shell
# tar(选项)(参数)
# -A或--catenate：新增文件到以存在的备份文件；
# -B：设置区块大小；
# -c或--create：建立新的备份文件；
# -C <目录>：这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项。
# -d：记录文件的差别；
# -x或--extract或--get：从备份文件中还原文件；
# -t或--list：列出备份文件的内容；
# -z或--gzip或--ungzip：通过gzip指令处理备份文件；
# -Z或--compress或--uncompress：通过compress指令处理备份文件；
# -f<备份文件>或--file=<备份文件>：指定备份文件；
# -v或--verbose：显示指令执行过程；
# -r：添加文件到已经压缩的文件；
# -u：添加改变了和现有的文件到已经存在的压缩文件；
# -j：支持bzip2解压文件；
# -v：显示操作过程；
# -l：文件系统边界设置；
# -k：保留原有文件不覆盖；
# -m：保留文件不被覆盖；
# -w：确认压缩文件的正确性；
# -p或--same-permissions：用原来的文件权限还原文件；
# -P或--absolute-names：文件名使用绝对名称，不移除文件名称前的“/”号；
# -N <日期格式> 或 --newer=<日期时间>：只将较指定日期更新的文件保存到备份文件里；
# --exclude=<范本样式>：排除符合范本样式的文件。
```
```shell
$ tar zcvf /tmp/etc.tar.gz  /etc/  # 将etc目录以gzip打包到/tmp/etc.tar.gz文件中 并显示执行过程
# zip(选项)(参数)
# -A：调整可执行的自动解压缩文件；
# -b<工作目录>：指定暂时存放文件的目录；
# -c：替每个被压缩的文件加上注释；
# -d：从压缩文件内删除指定的文件；
# -D：压缩文件内不建立目录名称；
# -f：此参数的效果和指定“-u”参数类似，但不仅更新既有文件，如果某些文件原本不存在于压缩文件内，使用本参数会一并将其加入压缩文件中；
# -F：尝试修复已损坏的压缩文件；
# -g：将文件压缩后附加在已有的压缩文件之后，而非另行建立新的压缩文件；
# -h：在线帮助；
# -i<范本样式>：只压缩符合条件的文件；
# -j：只保存文件名称及其内容，而不存放任何目录名称；
# -J：删除压缩文件前面不必要的数据；
# -k：使用MS-DOS兼容格式的文件名称；
# -l：压缩文件时，把LF字符置换成LF+CR字符；
# -ll：压缩文件时，把LF+cp字符置换成LF字符；
# -L：显示版权信息；
# -m：将文件压缩并加入压缩文件后，删除原始文件，即把文件移到压缩文件中；
# -n<字尾字符串>：不压缩具有特定字尾字符串的文件；
# -o：以压缩文件内拥有最新更改时间的文件为准，将压缩文件的更改时间设成和该文件相同；
# -q：不显示指令执行过程；
# -r：递归处理，将指定目录下的所有文件和子目录一并处理；
# -S：包含系统和隐藏文件；
# -t<日期时间>：把压缩文件的日期设成指定的日期；
# -T：检查备份文件内的每个文件是否正确无误；
# -u：更换较新的文件到压缩文件内；
# -v：显示指令执行过程或显示版本信息；
# -V：保存VMS操作系统的文件属性；
# -w：在文件名称里假如版本编号，本参数仅在VMS操作系统下有效；
# -x<范本样式>：压缩时排除符合条件的文件；
# -X：不保存额外的文件属性；
# -y：直接保存符号连接，而非该链接所指向的文件，本参数仅在UNIX之类的系统下有效；
# -z：替压缩文件加上注释；
# -$：保存第一个被压缩文件所在磁盘的卷册名称；
# -<压缩效率>：压缩效率是一个介于1~9的数值。

$ zip -r /tmp/etc.zip /etc/ # 将etc目录及其下所有子目录与文件打包到 /tmp/etc.zip文件中
```
#####3.单引号,双引号 反引号区别
```shell
# 单引号: 里面的特殊内容不会执行
# 双引号: 特殊内容会执行
# 反引号: 执行命令 等同于 $(cmd)
```
#####4.no space left on device 磁盘空间不足 原因 及 排查方法
```shell
# 1 正常满了, 加硬盘.
# 2 iNode数量不足, 查找较大文件适当清除
# 3 block 硬链接已经是0了, 但是相关调用的进程还在, 需要杀掉进程
# 硬链接数为零 进程调用数不为零 如何找到这样的文件

$ lsof | grep deleted 
rsyslogd   1250      root    1w      REG                8,3 4888889358     140789 /var/log/messages (deleted)

# lsof 显示出系统中被打开的文件 
# 第一列 软件/服务的名称
# 第八列 文件的大小 
# 第10列 文件的名字
# 第11列 标记（硬链接数为0 进程调用数不为零 就会有 delete)
# 重启对应的服务 释放磁盘空间 
$ /etc/init.d/rsyslog restart 
Shutting down system logger:                               [  OK  ]
Starting system logger:                                    [  OK  ]
```
#####5.软连接与硬链接区别
```shell
#    在Linux的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号inode 。软连接，其实就是新建立一个文件，这个文件就是专门用来指向别的文件的（那就和windows 下的快捷方式的那个文件有很接近的意味）。软链接产生的是一个新的文件，但这个文件的作用就是专门指向某个文件的，删了这个软连接文件，那就等于不需要这个连接，和原来的存在的实体原文件没有任何关系，但删除原来的文件，则相应的软连接不可用（cat那个软链接文件，则提示“没有该文件或目录“）

#    硬连接是不会建立inode的，他只是在文件原来的inode link count域再增加1而已，也因此硬链接是不可以跨越文件系统的。相反都是软连接会重新建立一个inode，当然inode的结构跟其他的不一样，他只是一个指明源文件的字符串信息。一旦删除源文件，那么软连接将变得毫无意义。而硬链接删除的时候，系统调用会检查inode link count的数值，如果他大于等于1，那么inode不会被回收。因此文件的内容不会被删除。
#    硬链接实际上是为文件建一个别名，链接文件和原文件实际上是同一个文件。可以通过ls -i来查看一下，这两个文件的inode号是同一个，说明它们是同一个文件；而软链接建立的是一个指向，即链接文件内的内容是指向原文件的指针，它们是两个文件。

#	软链接可以跨文件系统，硬链接不可以；软链接可以对一个不存在的文件名(filename)进行链接（当然此时如果你vi这个软链接文件，linux会自动新建一个文件名为filename的文件）,硬链接不可以（其文件必须存在，inode必须存在）；软链接可以对目录进行连接，硬链接不可以。两种链接都可以通过命令 ln 来创建。ln 默认创建的是硬链接。使用 -s 开关可以创建软链接。
```
#####6.linux文件删除原理
```shell
# linux 是通过link的数量控制文件删除的，只有当文件不存在任何链接时，该文件才会被删除，一般每个文件有两个link计数器： i_count 和 i_nlink

# i_count： 引用计数器，文件被一进程引用，i_count数增加 ，可以认为是当前文件使用者的数量； 
# i_nlink： 硬链接数目（可以理解为磁盘的引用计数器），创建硬链接对应的 i_nlink 就会增加

# 对于rm命令来说，实际就是减少磁盘的引用计数 i_nlink 。当 i_nlink 和 i_count 均为 0 时，文件才会被删除（这里的删除是指将文件名到 inode 的链接删除了，但文件在磁盘上的block数据块并未被删除）。
```
#####7./etc/passwd 和linux用户分类
```shell
# /etc/passwd 文件是系统的主要文件之一。该文件中包含了所有用户登录名清单；为所有用户指定了主目录；在登录时使用的 shell 程序名称等。该文件还保存了用户口令；给每个用户提供系统识别号。/etc/passwd 文件是一个纯文本文件，每行采用了相同的格式：name:password:uid:gid:comment:home:shell

# 这个文件是这样的，每一行代表一个账号，有几行就代表系统有几个账号。可以 用一行来分析一下其代表的含义（一共七部分，用:分隔开）：
# 账号名称：对应UID
# 密码：以前Linux的密码直接存放在该文件中，现在都存在/etc/shadow中了，存 入/etc/shadow就用x表示，如果这个地方是"!"，说明此用户不能用密码登录。
# UID：用户识别码（ID）当ID为0时说明账号是管理员身份，1到499就留给系统使用的 ，主要是一些系统服务，500到65535是留给一般用户的。
# GID：与/etc/group有关，就是用户初始化组的ID
# 用户信息说明栏目：解释用户
# 家目录：表示该用户的主文件夹，一般都是/home/xxx
# shell：shell脚本，一般默认都是BASH。

# 1.用户
#     用户是能够获取系统资源的权限的集合.
# 2.linux用户组的分类:
#     a.管理员root  : 具有使用系统所有权限的用户,其UID 为0.
#     b.普通用户  	 : 即一般用户,其使用系统的权限受限,其UID为500-60000之间.
#     c.系统用户    : 保障系统运行的用户,一般不提供密码登录系统,其UID为1-499之间.
```
#####8.rwx 还有权限计算 
```shell
# r（read）：可读取此文件的内容
# w（write）：可以编辑此文件内容（并不代表可以删除该文件）
# x（execute）：可以执行该文件
# 权限计算
# r = 4 
# w = 2 
# x = 1 
# - = 0
```
| 000  | 001  | 010  | 011  | 100  | 101  |
| ---- | ---- | ---- | ---- | ---- | ---- |
| —    | –x   | -w-  | -wx  | r–   | r-x  |
| 0    | 1    | 2    | 3    | 4    | 5    |
 ```shell
# 640：rw- r– —    7：— — rwx  755：rwx r-x r
 ```

