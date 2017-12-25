###### 1.无法远程连接到服务排查过程

首先要确保目标服务器的设置是否正确.

​	主要是selinux是否关闭, iptables是否有屏蔽

其次确定目标服务器网络是否通畅.

​	关键命令:ping 目标服务器域名/IP.

最后检查目标服务器的相应端口是否开放.

​	关键命令:telnet IP Port



1.创建一个目录/data 。  

```shell
mkdir /data
```

 2.在/data 下面建立一个文件oldboy.txt 。  

```shell
touch /data/oldboy.txt    
```

 3.为oldboy.txt 增加内容为“I am studying linux. ”。  

```shell
echo "I am studying linux"  > oldboy.txt
```

 4.把oldboy.txt 文件拷贝到/tmp 下。  

```shell
cp  /data/oldboy.txt  /tmp
```

 5.把/data  目录移动到/root 下。  

 ```shell
cp -R /data /root/
 ```

 6.进入/root  目录下的data  目录，删除oldboy.txt 文件。  

```shell
cd /root/data/ && rm oldboy.txt
```

 7.接第6 题，退出到上一级目录，删除data  目录。  

 ````shell
cd ../ && rm -rf data
 ````

8. 已知文件test.txt  内容为test  liyao oldboy  请给出输出test.txt 文件内容时，不包含oldboy 字符串的命令

```shell
grep -v "oldboy" test.txt
```

9. 请用一条命令完成创建目录/oldboy/test ，即创建/oldboy  目录及/oldboy/test  目录  

```shell
mkdir -p oldboy/test
```

10. 已知/tmp 下已经存在test.txt 文件，如何执行命令才能把/mnt/test.txt 拷贝到/tmp 下覆盖掉 

/tmp/test.txt ，而让系统不提示是否覆盖（root 权限下）。  

```shell
mv -f /mnt/test.txt /tmp
```

11. 只查看ett.txt 文件（共100 行）内第20 到第30 行的内容  

```shell
head -30 ett.txt | tail -11 #取前三十行后只看后11行
```

12. 分析图片服务日志，把日志（每个图片访问次数*图片大小的总和）排行，取 top10 ，也 

就是计算每个url 的总访问大小  

```shell
awk '{a[$7]++;$size[$7]+=$10}END{for(i in a) print size[i], a[i],i|"sort -k1 -nr | head -n10"}' access.log
# 这道题关键是需要看命令的语法怎么用, 思路到是很简单, 做两个计数器 一个记录url出现的次数 一个记录累加其大小 最后遍历输出两个数组的每一个键值对, 其最后的输出结果经由sort命令 以第一个列的倒序排序 并由head命令截取前十行显示
```

13. 把/oldboy       目录及其子目录下所有以扩展名.sh  结尾的文件中包含./hostlists.txt  的字符串 

全部替换为../idctest_iplist

```shell
sed -i 's#./hostlists.txt#../idctest_iplist#g' `find /oldboy -type f -name "*.sh" |xargs grep -l "./hostlists.txt"`
# 用find命令查找所有.sh结尾的文件 并由grep查找包含./hostlists.txt字符串的文件. find和grep组合查出所有包含指定字符串和后缀名的文件列表, 最后由sed执行批量替换
```



###### 3.命令总结-参数和例子

常用命令

cp 复制目录/文件  cp [-R]  /a /b/名字

mv 移动目录/文件 mv /a /b/名字

ll 查看文件列表

ls 同上

pwd  查看当前路径

head 从第一行向下查看指定行数的文件  head -10 test.txt

tail    从最后一行向上查看指定行数的文件  常用 tail -f test.log  跟踪日志用

cat    显示文件所用内容

more 配合其他命令使用 主要是内容太长不方便观看时使用.  cat  test.txt | more

chmod 更改文件权限

chown 更改文件用户.用户组

whereis 查找程序位置

find  查找 文件/文件夹位置 find 位置 类型 指定类型 名字(可以使用通配符)

grep 查找内容用

cut  截取显示

