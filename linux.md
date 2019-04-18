## Linux

### 1.Linux系统组成

**Linux内核** r.x.y (x次版本号，奇数开发版，偶数稳定版)

**Linux Shell**用户与内核进行交互的接口

​	内部命令：shell本身的命令

​	外部命令：可执行文件或shell脚本

shell搜索的目录名字都保存在PATH中，而Path保存在主目录中隐藏文件.profile或.login

​	shell变量：

​		内部变量：用户只能使用不能修改

​		用户变量：用户建立和修改

​		环境变量：决定用户工作环境，不需要用户去定义

**Linux应用程序**

**Linux文件系统**

### 2.通配符和转义字符

`* 多个(不能匹配隐藏文件)，`

`？单个，`

`[A-Z]任意一个`

\ 转义字符

### 3.标准输入、输出设备

> stdin 0 标准输入
>
> stdout 1 标准输出
>
> stderr 2 标准错误

重定向

> 输入重定向  <
>
> 输出重定向  >  >> 
>
> 错误重定向 2> 2>> 
>
> 同时输出重定向和错误重定向 &>

空设备（/dev/null）

​	这是一个黑洞，发往他的东西都将不复存在。

屏蔽命令的输出和错误输出

`myprogram &> /dev/null`

`myprogram > /dev/null 2>&1`

### 4.管道 |

`ls -l | grep "^d"` 将d开头的信息筛选出来

### 5.系统配置文件

```shell
文件系统配置文件/etc/fatab
用户配置文件/etc/passwd
	root:x:0:0:root:/root:/bin/bash
	nginx:x:996:994:Nginx web 
	用户名（也被称为登录名）,口令,UID ,GID（用户初始组）,用户名全称,家目录,SHELL类型
用户口令/etc/shadow
组账号/etc/group
	nginx:x:994:
	root:x:0:linuxsir
	用户组名称，用户组密码，GID，用户列表（本字段如果为空，则为初始组；表示附加组；GID相同的用户）
组口令/etc/gshadow
```

### 6.vi编辑器

普通模式 `ESC`

编辑模式 `i`

末行模式 `:`

### 7.存储挂载

必须要将存储设备挂载到linux目录树上。

在系统启动过程中，根据记录在`/etc/fstab`文件，确定并按照文件系统，形成初始目录树

```shell
#mount -t vfat /dev/had1 /mnt/c
#mount /dev/had1 /mnt/c
         文件系统类型 设备名称  挂载目录
       ide磁盘（scsi/usb--sd,软盘fd）的第一块盘，第一个分区(前4个位主分区或扩展分区，第5个位逻辑分区)
         vfat（fat32） ntfs（ntfs文件系统）	ext4(linux文件系统) iso9660(cd-rom)
#mount -o iocharset=cp936 /dev/sda1 /mnt/usb 
#mount -o ro /dev/hda3 /mnt/usr
		参数=值
卸载命令
#umount /mnt/usb
查看存储设备
#fdisk -l
```

### 8.文件操作命令

```shell
文件查看和连接
#cat file1 file2 > file3
分屏显示
#more -c -10 file1
	每10行显示一次，之前显示的清屏
按页显示
#less file1
复制
#cp source dest
	-R/-r 递归复制  -f强制复制  -i交互式复制
删除
#rm file
移动或重命名
#mv source dest

软链接
#ln -s source dest
可跨越文件系统，若链接指向的文件从一个目录移动到另一个目录，就无法通过软连接访问，存inode信息

硬链接
#ln source dest
不可跨越文件系统、只有超级用户才可、不占用空间

查找文件
and -a,or -o,not !
#find path -name "main" -o -name "mina*"

文件内容检索grep
#grep smith /phonebook
#grep ‘^s’ /mybook
#ls | grep \*$
	显示所有以*号结尾的文件

改变文件、目录时间命令
#touch -t 201904311200 file

目录操作
#mkdir -p /dir
#rmdir -p /dir

ls
#ls -a 显示所有文件
#ls -l 长格式列表
-rw-r--r-- 1 root root 191753373 Nov 27 19:12 jdk
-普通文件 b块文件 c字符文件 d目录 l链接 p命名管道
#ls -R 

权限
#chmod 
	改变文件或目录的权限
	ugoa  +-=   rwx-
	644 
#chown
	改变文件或目录的所有者
	
#chgrp
	改变文件或目录所属的组

gzip
#gzip -v f1.gz f2.gz 压缩文件
#gzip -d f.gz  解压文件
zip
#zip f1.zip f1 将f1压缩为f1.zip
#unzip f1.zip 解压文件

tar
#tar cvf f1.tar /dir 
f指定打包名字(必须放在最后)
z 是gzip打包的文件，c创建，v过程，x解压，t列出内容
#tar -zxvf mybal.tar.gz
```

### 9.进程启动方式

> 前台启动：普通命令
>
> 后台启动：命令后加&



调度启动：安装预先指定时间执行；

- at 安排延时任务
- batch 低优先级执行
- cron 寻找以用户命名的crontab文件，以用户身份去运行该文件



### 10.gcc使用

```shell
生成预处理文件
#gcc -E hello.c -o hello.i
生成汇编文件
#gcc -s hello.c -o hello.s
汇编生成目标代码文件
#gcc -c hello.c -o hello.o
链接生成可执行文件
#gcc hello.c -o hello
运行
#./hello
```

