# 如意吉祥的Linux成长心路历程（三）  
开课一个月了，知识一点点由浅入深，明显感觉学习的节奏让我有些不太适应，光凭课上听讲理解是远远不够了，课上需要反复听视频里面的知识点并记录下来，如下较重要的知识记录下来，以便日后查询。

# 软件运行和编译  

* ABI：Application Binary Interface  
Windows与Linux不兼容  
ELF(Executable and Linkable Format)  
PE（Portable Executable）  
库级别的虚拟化：  
Linux: WINE    
Windows: Cygwin  
* API：Application Programming Interface  
POSIX：Portable OS  
程序源代码--> 预处理--> 编译--> 汇编--> 链接  
静态编译：.a  
动态编译：.so  

# 静态和动态链接  

链接主要作用是把各个模块之间相互引用的部分处理好，使得各个模块之间能够正确地衔接，分为静态链接和动态链接  
* 静态链接  
把程序对应的依赖库复制一份到包  
libxxx.a  
嵌入程序包  
升级难，需重新编译  
占用较多空间，迁移容易  
* 动态链接  
只把依赖加做一个动态链接  
libxxx.so  
连接指向  
占用较少空间，升级方便  

# 包管理器
* 二进制应用程序的组成部分：
二进制文件、库文件、配置文件、帮助文件
* 程序包管理器：  
debian：deb文件, dpkg包管理器  
redhat：rpm文件, rpm包管理器  
rpm：RedhatPackage Manager  
RPM Package Manager  

# 库文件
* 查看二进制程序所依赖的库文件  
```
ldd/PATH/TO/BINARY_FILE  
```
管理及查看本机装载的库文件  
ldconfig加载库文件  
/sbin/ldconfig-p: 显示本机已经缓存的所有可用库文件名及文件
路径映射关系  
配置文件：/etc/ld.so.conf, /etc/ld.so.conf.d/*.conf  
缓存文件：/etc/ld.so.cache


# rpm包管理
* CentOS系统上使用rpm命令管理程序包：  
安装、卸载、升级、查询、校验、数据库维护  
安装：
```
rpm {-i|--install} [install-options] PACKAGE_FILE…
-v: verbose
-vv:
-h: 以#显示程序包管理执行进度
rpm -ivhPACKAGE_FILE ...
```
# rpm包安装
```
[install-options]
--test: 测试安装，但不真正执行安装，即dry run模式
--nodeps：忽略依赖关系
--replacepkgs| replacefiles
--nosignature: 不检查来源合法性
--nodigest：不检查包完整性
--noscripts：不执行程序包脚本
%pre: 安装前脚本--nopre
%post: 安装后脚本--nopost
%preun: 卸载前脚本--nopreun
%postun: 卸载后脚本--nopostun
```
# rpm包升级
```
rpm {-U|--upgrade} [install-options] PACKAGE_FILE...  
rpm {-F|--freshen} [install-options] PACKAGE_FILE...  
upgrade：安装有旧版程序包，则“升级”  
如果不存在旧版程序包，则“安装”  
freshen：安装有旧版程序包，则“升级”  
如果不存在旧版程序包，则不执行升级操作  
rpm -UvhPACKAGE_FILE ...  
rpm -FvhPACKAGE_FILE ...  
--oldpackage：降级  
--force: 强制安装  
```

# 包查询
```
rpm {-q|--query} [select-options] [query-options]  
[select-options]  
-a: 所有包  
-f: 查看指定的文件由哪个程序包安装生成  
-p rpmfile：针对尚未安装的程序包文件做查询操作  
--whatprovidesCAPABILITY：查询指定的CAPABILITY由哪个包所提供  
--whatrequiresCAPABILITY：查询指定的CAPABILITY被哪个包所依赖  
rpm2cpio 包文件|cpio–itv预览包内文件  
rpm2cpio 包文件|cpio–id “*.conf”释放包内文件  
```
```
[query-options]
--changelog：查询rpm包的changelog
-c: 查询程序的配置文件
-d: 查询程序的文档
-i: information
-l: 查看指定的程序包安装后生成的所有文件
--scripts：程序包自带的脚本
--provides: 列出指定程序包所提供的CAPABILITY
-R: 查询指定的程序包所依赖的CAPABILITY
```

* 常用查询用法：  
-qi PACKAGE, -qfFILE, -qc PACKAGE, -qlPACKAGE   -qdPACKAGE  
-qpiPACKAGE_FILE, -qplPACKAGE_FILE, ...  
-qa  
* 包卸载：
```
rpm {-e|--erase} [--allmatches] [--nodeps] [--noscripts] [--notriggers] [--test] PACKAGE_NAME ...  
```

# 包校验
```
rpm {-V|--verify} [select-options] [verify-options]
S file Size differs
M Mode differs (includes permissions and file type)
5 digest (formerly MD5 sum) differs
D Device major/minor number mismatch
L readLink(2) path mismatch
U User ownership differs
G Group ownership differs
T mTimediffers
P capabilities differ
```

* 导入所需要公钥
```
rpm -K|checksigrpmfile检查包的完整性和签名
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

# rpm数据库
数据库重建：  
```
/var/lib/rpm
rpm {--initdb|--rebuilddb}
initdb: 初始化
如果事先不存在数据库，则新建之
否则，不执行任何操作
rebuilddb：重建已安装的包头的数据库索引目录
```

# yum
CentOS: yum, dnf  
YUM: YellowdogUpdate Modifier，rpm的前端程序，可解决软件包相关依赖性，可在多个库之间定位软件包，up2date的替代工具  
yum repository: yum repo，存储了众多rpm包，以及包的相关的元数据文件（放置于特定目录repodata下）  
文件服务器：  
http://  
https://  
ftp://  
file://  

# yum配置文件
* yum客户端配置文件：  
/etc/yum.conf：为所有仓库提供公共配置  
/etc/yum.repos.d/*.repo：为仓库的指向提供配置  
* 仓库指向的定义：  
[repositoryID]  
>name=Some name for this repository  
baseurl=url://path/to/repository/  
enabled={1|0}  
gpgcheck={1|0}  
gpgkey=URL  
enablegroups={1|0}  
failovermethod={roundrobin|priority}  
roundrobin：意为随机挑选，默认值   
priority:按顺序访问  
cost= 默认为1000  
>

# yum仓库
* yum的repo配置文件中可用的变量：  
$releasever: 当前OS的发行版的主版本号  
$arch: 平台，i386,i486,i586,x86_64等  
$basearch：基础平台；i386, x86_64  
$YUM0-$YUM9:自定义变量  


# yum命令
```
*yum命令的用法：  
yum [options] [command] [package ...]  

* 显示仓库列表：  
yum repolist[all|enabled|disabled]  

* 显示程序包：  
yum list  
yum list [all | glob_exp1] [glob_exp2] [...]  
yum list {available|installed|updates} [glob_exp1] [...]

* 安装程序包：  
yum install package1 [package2] [...]  
yum reinstall package1 [package2] [...] (重新安装)  

*升级程序包：
yum update [package1] [package2] [...]
yum downgrade package1 [package2] [...] (降级)

*检查可用升级：
yum check-update

*卸载程序包：
yum remove | erase package1 [package2] [...]

*升级程序包：
yum update [package1] [package2] [...]
yum downgrade package1 [package2] [...] (降级)

*检查可用升级：
yum check-update

*卸载程序包：
yum remove | erase package1 [package2] [...]


*查看程序包information：
yum info [...]

*查看指定的特性(可以是某文件)是由哪个程序包所提供：
yum provides | whatprovidesfeature1 [feature2] [...]

*清理本地缓存：
清除/var/cache/yum/$basearch/$releasever缓存
yum clean [ packages | metadata | expire-cache | rpmdb| plugins | all ]

*构建缓存：
yum makecache

*搜索：yum search string1 [string2] [...]
    以指定的关键字搜索程序包名及summary信息

*查看指定包所依赖的capabilities：
yum deplistpackage1 [package2] [...]

*查看yum事务历史：
yum history [info|list|packages-list|packages-info|

*安装及升级本地程序包：
yum localinstall rpmfile1 [rpmfile2] [...]
(用install替代)
yum localupdate rpmfile1 [rpmfile2] [...]
(用update替代)
```
* 包组管理的相关命令：   
yum groupinstall group1 [group2] [...]  
yum groupupdate group1 [group2] [...]  
yum grouplist [hidden] [groupwildcard] [...]  
yum groupremove group1 [group2] [...]  
yum groupinfo group1 [...]  

*yum的命令行选项：
```
--nogpgcheck：禁止进行gpgcheck
-y: 自动回答为“yes”
-q：静默模式
--disablerepo=repoidglob：临时禁用此处指定的repo
--enablerepo=repoidglob：临时启用此处指定的repo
--noplugins：禁用所有插件
```

