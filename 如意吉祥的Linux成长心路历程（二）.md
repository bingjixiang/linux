#如意吉祥的Linux成长心路历程（二）
===============================
 转眼间半个月的学习时光过去了，相比较当时怀着激动而又忐忑的心情上课，现在的心态已经平稳了许多，也许是知识点 太多又或者是别的什么，现在愈发感觉时间不够用了，好多知识点都需要花费很多时间去复习巩固，接下来我就把上周学习的内容总结下来，以备今后便于查漏补缺。  
 
 * 文件通配符  
 ------------  
***匹配零个或多个字符**  
?**匹配任何单个字符**  
~ **当前用户家目录**  
~**mage 用户mage家目录**  
~+ **当前工作目录**  
~-**前一个工作目录**  
[0-9]**匹配数字范围**  
[a-z]：**字母**  
[A-Z]：**字母**  
[bing]**匹配列表中的任何的一个字符**  
[^bing]**匹配列表中的所有字符以外的字符**  
  
  预定义字符类 
[:digit:]：**任意数字，相当于0-9**  
[:lower:]：**任意小写字母**  
[:upper:]: **任意大写字母**  
[:alpha:]: **任意大小写字母**  
[:alnum:]：**任意数字或字母**  
[:blank:]：**水平空白字符**  
[:space:]：**水平或垂直空白字符**  
[:punct:]：**标点符号**  
[:print:]：**可打印字符**  
[:cntrl:]：**控制（非打印）字符**  
[:graph:]：**图形字符**  
[:xdigit:]：**十六进制字符**  
  
* 创建空文件和刷新时间  
-----------------------  
格式：touch [OPTION]... FILE...  
-a **仅改变atime和ctime**   
-m **仅改变mtime和ctime**  
-t **[[CC]YY]MMDDhhmm[.ss]  指定atime和mtime的时间戳**  
-c **如果文件不存在，则不予创建**

* 复制文件CP
-------------  
-i：**覆盖前提示–n:不覆盖，注意两者顺序**  
-r, -R: **递归复制目录及内部的所有内容**     
-a: **归档，相当于-dR--preserv=all**  
-d：**--no-dereference --preserv=links 不复制原文件，只复制链接名**  
-p: **等同--preserv=mode,ownership,timestamp**  
-v: **--verbose**  
-f: **--force**  
-u:**--update 只复制源比目标更新文件或目标不存在的文件**  
-b:**目标存在，覆盖前先备份**  
**--backup=numbered 目标存在，覆盖前先备份加数字后缀**  

* 移动和重命名文件  
------
mv [OPTION]... [-T] SOURCE DEST 
mv [OPTION]... SOURCE... DIRECTORY  
mv [OPTION]... -t DIRECTORY SOURCE...  
常用选项：  
-i: **交互式**  
-f: **强制**  
-b: **目标存在，覆盖前先备份**  

* 删除  
-----
rm[OPTION]... FILE...  
常用选项：  
-i:**交互式**  
-f:**强制删除**  
-r:**递归**   

* 目录操作
----
* tree 显示目录树  
-d:**只显示目录**  
-L level:**指定显示的层级数目**  
-P pattern:**只显示由指定pattern匹配到的路径**  
mkdir创建目录  
-p:**存在于不报错，且可自动创建所需的各目录**  
-v:**显示详细信息**  
-m MODE:**创建目录时直接指定权限**  

* rmdir删除空目录  
-p:**递归删除父空目录**  
-v:**显示详细信息**  
-r：**递归删除目录树**  


* inode表结构与文件关系
------

![inode](http://m.qpic.cn/psb?/V12PYWW41RFPlV/x*ZeGEOZpL1iAxNO.Eq6u92TVbNjX7QTKfgo8UVJRyw!/b/dDYBAAAAAAAA&bo=ewTOAgAAAAADJ7E!&rf=viewer_4)

文件引用一个是inode号  
人是通过文件名来引用一个文件  
一个目录是目录下的文件名和文件inode号之间的映射  

* cp和inode
---  
在CP的命令：  
分配一个空闲的inode号，在inode表中生成新条目
在目录中创建一个目录项，将名称与inode编号关联
拷贝数据生成新的文件

* rm和inode
----  
rm命令：  
链接数递减，从而释放的inode号可以被重用  
把数据块放在空闲列表中  
删除目录项  
数据实际上不会马上被删除，但当另一个文件使用数据块时将被覆盖  

* mv和inode  
---
**如果mv命令的目标和源在相同的文件系统**  
用新的文件名创建对应新的目录项  
删除旧目录条目对应的旧的文件名  
不影响inode表（除时间戳）或磁盘上的数据位置：没有数据被移动       
**如果目标和源在一个不同的文件系统，mv相当于cp和rm**


* 硬链接与软连接
---
1.硬链接  
创建硬链接会增加额外的记录项以引用文件  
对应于同一文件系统上一个物理文件  
每个目录引用相同的inode号  
创建时链接数递增  

删除文件时：  
rm命令递减计数的链接  
文件要存在，至少有一个链接数  
当链接数为零时，该文件被删除  
不能跨越驱动器或分区  
语法:  
_ln filename [linkname]_

2.软连接  
一个符号链接指向另一个文件  
ls -l的显示链接的名称和引用的文件  
一个符号链接的内容是它引用文件的名称  
可以对目录进行  
可以跨分区  
指向的是另一个文件的路径；其大小为指向的路径字符串的长度；不增加或减少目标文件inode的引用计数  
语法：
_ln -s filename [linkname]_

---
* 文件的输入与输出  
---
*管  道     
管道（使用符号“|”表示）用来连接命令  
命令1 | 命令2 | 命令3 | …  
将命令1的STDOUT发送给命令2的STDIN，命令2的STDOUT发送到命令3的STDIN  
STDERR默认不能通过管道转发，可利用2>&1 或|& 实现最后一个命令会在当前shell进程的子shell进程中执行用来  

例：  
less ：一页一页地查看输入  

>ls-l/etc|less  

mail：通过电子邮件发送输入  

>echo"testemail"|mail-s "test"user@example.com  

lpr：把输入发送给打印机  
>echo"testprint"|lpr-Pprinter_name  

---

* 用户和组管理
---
**用户创建：useradd**  
useradd[options] LOGIN  
-u UID  
-o 配合-u 选项，不检查UID的唯一性  
-g GID：指明用户所属基本组，可为组名，也可以GID  
-c "COMMENT"：用户的注释信息  
-d HOME_DIR: 以指定的路径(不存在)为家目录  
-s SHELL: 指明用户的默认shell程序，可用列表在/etc/shells文件中  
-G GROUP1[,GROUP2,...]：为用户指明附加组，组须事先存在  
-N 不创建私用组做主组，使用users组做主组   
-r: 创建系统用户CentOS 6: ID<500，CentOS 7: ID<1000  
-m 创建家目录，用于系统用户  
-M 不创建家目录，用于非系统用户  

**用户属性修改**  
usermod[OPTION] login  
-u UID: 新UID  
-g GID: 新主组  
-G GROUP1[,GROUP2,...[,GROUPN]]]：新附加组，原来的附加组将会被覆盖；若保留原有，则要同时使用-a选项  
-s SHELL：新的默认SHELL  
-c 'COMMENT'：新的注释信息  
-d HOME: 新家目录不会自动创建；若要创建新家目录并移动原家数  据，同时使用-m选项  
-l login_name: 新的名字；  
-L: lock指定用户,在/etc/shadow 密码栏的增加!  
-U: unlock指定用户,将/etc/shadow 密码栏的! 拿掉  
-e YYYY-MM-DD: 指明用户账号过期日期   
-f INACTIVE: 设定非活动期限  

**删除用户**  
userdel[OPTION]... login  
-r: 删除用户家目录  

**查看用户相关的ID信息**  
id [OPTION]... [USER]  
-u: 显示UID  
-g: 显示GID  
-G: 显示用户所属的组的ID  
-n: 显示名称，需配合ugG使用  
 
**切换用户或以其他用户身份执行命令**  
su[options...] [-] [user [args...]]  
切换用户的方式：  
suUserName：非登录式切换，即不会读取目标用户的配置文件，不改变当前工作目录  
su-UserName：登录式切换，会读取目标用户的配置文件，切换至家目录，完全切换  
root su至其他用户无须密码；非root用户切换时需要密码  
换个身份执行命令：  
su[-] UserName-c 'COMMAND'  
选项：-l --login  
su-l UserName相当于su-UserName  

**设置密码**  
passwd[OPTIONS] UserName: 修改指定用户的密码  
常用选项：  
-d：删除指定用户密码  
-l：锁定指定用户  
-u：解锁指定用户  
-e：强制用户下次登录修改密码  
-f：强制操作  
-n mindays：指定最短使用期限  
-x maxdays：最大使用期限  
-w warndays：提前多少天开始警告  
-iinactivedays：非活动期限  
--stdin：从标准输入接收用户密码  
>echo "PASSWORD" | passwd--stdinUSERNAME  

**修改用户密码策略**  
chage[OPTION]... LOGIN   
-d LAST_DAY  
-E --expiredateEXPIRE_DATE  
-I --inactive INACTIVE  
-m --mindaysMIN_DAYS  
-M --maxdaysMAX_DAYS  
-W --warndaysWARN_DAYS  
–l 显示密码策略  

**创建组**  
groupadd[OPTION]... group_name  
-g GID: 指明GID号；[GID_MIN, GID_MAX]  
-r: 创建系统组  
CentOS 6: ID<500
CentOS 7: ID<1000  

**修改和删除组**  
组属性修改：groupmod  
groupmod[OPTION]... group  
-n group_name: 新名字  
-g GID: 新的GID  
组删除：groupdel  groupdelGROUP  

**更改组密码**  
组密码：gpasswd  
gpasswd[OPTION] GROUP  
-a user 将user添加至指定组中  
-d user 从指定组中移除用户user  
-A user1,user2,... 设置有管理权限的用户列表  
newgrp命令：临时切换主组  
如果用户本不属于此组，则需要组密码   

**更改和查看组成员**
groupmems[options] [action]  
options：  
-g, --group groupname更改为指定组(只有root)  
Actions:  
-a, --add username 指定用户加入组  
-d, --delete username 从组中删除用户  
-p, --purge 从组中清除所有成员  
-l, --list 显示组成员列表  
groups [OPTION].[USERNAME]... 查看用户所属组列表  


---
* 文件权限  
---
文件的权限主要针对三类对象进行定义  
owner: 属主, u  
group: 属组, g  
other: 其他, o  
每个文件针对每类访问者都定义了三种权限  
r: Readable  
w: Writable  
x: eXcutable  

![rooot](http://m.qpic.cn/psb?/V12PYWW41RFPlV/R6QhDSW9y7tvbOZIZBabiP8ytPOh7iDN7iTZH63GCBw!/b/dDQBAAAAAAAA&bo=TwPQAAAAAAADB74!&rf=viewer_4)

**修改文件权限**

chmod[OPTION]... OCTAL-MODE FILE...  
-R: 递归修改权限  
chmod[OPTION]... MODE[,MODE]... FILE...  
MODE：  
修改一类用户的所有权限：  
u= g= o= ug= a= u=,g=  
修改一类用户某位或某些位权限  
u+ u-g+ g-o+ o-a+ a-+ -  
chmod[OPTION]... --reference=RFILE FILE...  
参考RFILE文件的权限，将FILE的修改为同RFILE  

----
**访问控制列表**

---  
ACL：Access Control List，实现灵活的权限管理  
除了文件的所有者，所属组和其它人，可以对更多的用户设置权限  
CentOS7默认创建的xfs和ext4文件系统具有ACL功能  
CentOS7之前版本，默认手工创建的ext4文件系统无ACL功能,需手动增加  
tune2fs –o acl/dev/sdb1  
mount –o acl/dev/sdb1 /mnt/test  
ACL生效顺序：所有者，自定义用户，自定义组，其他人  

为多用户或者组的文件和目录赋予访问权限rwx  
•mount -o acl/directory  
•getfaclfile |directory  
•setfacl-m u:wang:rwx file|directory  
•setfacl-Rm g:sales:rwX directory  
•setfacl-M file.aclfile|directory  
•setfacl-m g:salesgroup:rw file| directory  
•setfacl-m d:u:wang:rx directory  
•setfacl-x u:wang file |directory  
•setfacl-X file.acldirectory  

ACL文件上的group权限是mask 值（自定义用户，自定义组，拥有组的最大权限）,而非传统的组权限  
getfacl可看到特殊权限：flags  
通过ACL赋予目录默认x权限，目录内文件也不会继承x权限  
base ACL 不能删除  
setfacl-k dir 删除默认ACL权限  
setfacl–b file1清除所有ACL权限  
getfaclfile1 | setfacl--set-file=-file2 复制file1的acl权限给file2  

备份和恢复ACL  
主要的文件操作命令cp和mv都支持ACL，只是cp命令需要加上-p 参数。但是tar等常见的备份工具是不会保留目录和文件的ACL信息  
getfacl-R /tmp/dir1 > acl.txt  
setfacl -R -b /tmp/dir1  
setfacl-R --set-file=acl.txt/tmp/dir1  
setfacl--restore acl.txt  
getfacl -R /tmp/dir1  

---
**抽取文本的工具**  

--------
* 文件查看  
文件查看命令：  
cat，tac，rev  
cat [OPTION]... [FILE]...  
-E：显示行结束符$  
-n：对显示出的每一行进行编号  
-A：显示所有控制符  
-b：非空行编号  
-s：压缩连续的空行成一行  
tac：纵向反向显示  
rev：横向反向显示  

* 分页查看文件内容  
more: 分页查看文件  
more [OPTIONS...] FILE...   
-d: 显示翻页及退出提示  
less：一页一页地查看文件或STDIN输出  
查看时有用的命令包括：  
/文本搜索文本   
n/N跳到下一个或上一个匹配  
less命令是man命令使用的分页器    

* 显示文本前或后行内容 
head [OPTION]... [FILE]...  
-c #: 指定获取前#字节  
-n #: 指定获取前#行  
-#：指定行数  
tail [OPTION]... [FILE]...  
-c #: 指定获取后#字节  
-n #: 指定获取后#行  
-#：同上  
-f: 跟踪显示文件fd新追加的内容,常用日志监控  
相当于--follow=descriptor  
-F: 跟踪文件名，相当于--follow=name --retry  
tailf类似tail –f，当文件不增长时并不访问文件  

* 按列抽取文本cut和合并文件paste  
cut [OPTION]... [FILE]...  
-d DELIMITER: 指明分隔符，默认tab  
-f FILEDS:  
#: 第#个字段  
#,#[,#]：离散的多个字段，例如1,3,6  
#-#：连续的多个字段, 例如1-6  
混合使用：1-3,7  
-c按字符切割  
--output-delimiter=STRING指定输出分隔符  

* 收集文本统计数据wc  
计数单词总数、行总数、字节总数和字符总数  
可以对文件或STDIN中的数据运行  
wc story.txt  
39 237 1901 story.txt  
行数字数字节数  
常用选项  
-l只计数行数  
-w只计数单词总数  
-c只计数字节总数  
-m只计数字符总数  
-L显示文件中最长行的长度    

* 文本排序sort    
把整理过的文本显示在STDOUT，不改变原始文件  
sort[options]file(s)  
常用选项  
-r执行反方向（由上至下）整理  
-R随机排序  
-n执行按数字大小整理  
-f选项忽略（fold）字符串中的字符大小写  
-u选项（独特，unique）删除输出中的重复行  
-t c选项使用c做为字段界定符  
-k X选项按照使用c字符分隔的X列来整理能够使用多次

uniq  
uniq命令：从输入中删除前后相接的重复的行  
uniq[OPTION]... [FILE]...  
-c: 显示每行重复出现的次数  
-d: 仅显示重复过的行  
-u: 仅显示不曾重复的行  
注：连续且完全相同方为重复  
常和sort 命令一起配合使用：  
>sort userlist.txt | uniq-c  
