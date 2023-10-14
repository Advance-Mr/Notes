#                                                                                           linux

查看ununt的发行版本：lsb_release -a

## 1  安装GUI

### 1.1  ***CentOS安装***

CentOS minimal环境安装图形界面。

- ​     列出所有可用的Environment Groups


```
yum group list
yum groupinfo "GNOME Desktop"
```




- ​     选择GNOME Desktop软件包组进行安装

```
  yum groupinstall -y 'GNOME Desktop'
```




- 如果要通过GUI配置网络需要安装Server with GUI

```
yum groups install -y "Server with GUI"
```




- 修改default target启用GUI

```
systemctl set-default graphical.target
systemctl get-default
```

-  重新系统可直接进入图形界面  ：  reboot

-  删除GUI ，命令：                            yum groupremove "GNOME Desktop"

### 1.2  ***ubuntu安装***

  1.更新一下 ：sudo apt-get update
  2.安装ubuntu桌面图形化显示 ：sudo apt-get install ubuntu-desktop
  3.安装unity桌面系统：sudo apt-get install unity
  4.安装lightdm显示管理器：sudo apt-get install lightdm
  5.选择 lightdm或者gdm3都可以：sudo service gdm3 start
  6.设置ubuntu开机的默认开启方式为图形化界面显示：sudo systemctl set-default graphical.target
  7.重启系统：reboot
  8.特殊情况

​     再次安装图形化界面显示：sudo apt-get install ubuntu-desktop
​     输入命令：startx 

## 2  防火墙

###  2.1    [CentOS7]防火墙

**1.   使用firewalld开启关闭防火墙与端口**

​           关闭防火墙：    systemctl stop firewalld.service

​           开启防火墙：    systemctl start firewalld.service

若遇到无法开启

​          先用：systemctl unmask firewalld.service 
​          然后：systemctl start firewalld.service

**2.   启动与关闭**

- 开启开机启动：

  systemctl enable firewalld.service

- 关闭开机启动：

  systemctl disable firewalld.service

**3.  查看防火墙状态： **   systemctl status firewalld 

**4.  重启防火墙（重新载入，更新配置）**：  firewall-cmd --reload

**5.  systemctl**
     systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。

```
启动一个服务：  systemctl start firewalld.service
关闭一个服务：  systemctl stop firewalld.service
重启一个服务：  systemctl restart firewalld.service
显示一个服务的状态：  systemctl status firewalld.service
在开机时启用一个服务：  systemctl enable firewalld.service
在开机时禁用一个服务：  systemctl disable firewalld.service
查看服务是否开机启动：  systemctl is-enabled firewalld.service
查看已启动的服务列表：  systemctl list-unit-files|grep enabled
查看启动失败的服务列表：  systemctl --failed
```

------



### 2.2   [CentOS6]防火墙

 存在以下两种方式：

***一、  service方式***

**1.  查看防火墙状态**：   [root@centos6 ~]# service iptables status
                                                                        iptables：未运行防火墙。
**2.  开启与关闭**
开启防火墙：  [root@centos6 ~]#    service iptables start

关闭防火墙：  [root@centos6 ~]#    service iptables stop

​             会重定向到“/bin/systemctl stop iptables.service”

***二、iptables方式***
先进入init.d目录，命令如下：

```
[root@centos6 ~]# cd /etc/init.d/

[root@centos6 init.d]#
```

**1.  然后查看防火墙状态：**  [root@centos6 init.d]#    /etc/init.d/iptables status

**2.暂时关闭防火墙：** [root@centos6 init.d]#    /etc/init.d/iptables stop

**3.重启iptables**：  [root@centos6 init.d]#      /etc/init.d/iptables restart

------



### 2.3   [ubuntu]防火墙

**1.  安装防火墙：**    sudo apt-get install ufw

**2.  开启防火墙**

```bash
sudo ufw enable 
sudo ufw default deny 
```

   #运行以上两条命令后，开启了防火墙，并在系统启动时自动开启。
   #关闭所有外部对本机的访问，但本机访问外部正常。

**3.  关闭防火墙**：    sudo ufw disable

**4.  查看防火墙状态：**  sudo ufw status

*#补充：开启/关闭防火墙 (默认设置是’disable’)*

## 3  配置ssh

***一、 在SSH服务器所在机器上***

1、以root用户登录，更改ssh配置文件 /etc/ssh/sshd_config，去除以下配置的注释

```bash
RSAAuthentication yes #启用rsa认证
PubkeyAuthentication yes #启用公钥私钥配对认证方式
AuthorizedKeysFile .ssh/authorized_keys #公钥文件路径
```

2、重启SSH服务

```bash
[root@server /]#systemctl restart sshd  //重启ssh服务
```

------

***二、  在客户端机器上***

方法一：推荐

1、服务器 master 上生成密钥

​          密钥命令：        ssh-keygen -t rsa          直接按三次回车，生成.ssh文件

​          查看文件内容命令：       cd  .ssh/

```
-     authorized_keys: 存放远程免密登录的公钥,主要通过这个文件记录多台机器的公钥。
-     id_rsa: 生成的私钥文件
-     id_rsa.pub: 生成的公钥文件
-      known_hosts: 已知的主机公钥清单
```



2、远程密钥登录

这里介绍最常用的二种方式，一是通过 ssh-copy-id 命令，二是通过 scp 命令

 

- 通过 ssh-copy-id 命令设置。最后一个参数是我们要免密钥登录的服务器 ip 地址。                 

```
ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.1.100
```



- 通过 scp 命令直接将该文件远程复制过去，需要注意，如果你之前已经配置了其它服务器上的密钥，这是使用这种方法，就 

  会覆盖掉你原来的密钥，这时候是不建议使用这种方式的。如果你是先将该文件复制到服务器上的一个目录下，然后在使用     

   追加方式，将密钥追加到 authorized_keys 也是完全 OK 的。如果你只有两台服务器也是可以直接复制到文件。

```
  scp -p ~/.ssh/id_rsa.pub root@<ip>:/root/.ssh/authorized_keys
```

​       最后验证命令： ssh <u>ip</u>

------

方法二：

1、生成公钥私钥对

```bash
[root@client /]#ssh-keygen -t rsa
```

一路默认回车，四个回车，系统在/root/.ssh下生成`id_rsa`、`id_rsa.pub`

这里使用的`rsa`还可以指定其他算法如：dsa | ecdsa | ed25519 | rsa1z。不同版本的系统支撑的算法可能不一样。详情请使用`man ssh-keygen`查看。

2、把`id_rsa.pub`发送到  <u>服务端</u> （其他的非服务器端的客户机） 机器上

```bash
[root@client /]#ssh-copy-id -i /root/.ssh/id_rsa.pub root@192.168.11.20 #server ip
```

这里传个了192.168.11.20主机的root用户，你可以替换成其他用户。

3、验证

```bash
[root@client /]#ssh root@192.168.11.20 #server ip
```

------



## 4  复制

###     4.1远程scp

命令参数：

```
-1 强制scp命令使用协议ssh1 

-2 强制scp命令使用协议ssh2 

-4 强制scp命令只使用IPv4寻址 

-6 强制scp命令只使用IPv6寻址 

-B 使用批处理模式（传输过程中不询问传输口令或短语） 

-C 允许压缩。（将-C标志传递给ssh，从而打开压缩功能） 

-p 保留原文件的修改时间，访问时间和访问权限。 

-q 不显示传输进度条。 

-r 递归复制整个目录。 

-v 详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。  

-c cipher 以cipher将数据传输进行加密，这个选项将直接传递给ssh。  

-F ssh_config 指定一个替代的ssh配置文件，此参数直接传递给ssh。 

-i identity_file 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。  

-l limit 限定用户所能使用的带宽，以Kbit/s为单位。   

-o ssh_option 如果习惯于使用ssh_config(5)中的参数传递方式，  

-P port 注意是大写的P, port是指定数据传输用到的端口号  

-S program 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。
```

#### **4.1.2  复制文件**

   (1) 复制文件： 

​        命令格式： 

```
   scp local_file remote_username@remote_ip:remote_folder 
    或
   scp local_file remote_username@remote_ip:remote_file 
    或
   scp local_file remote_ip:remote_folder 
    或
   scp local_file remote_ip:remote_file 
```

​      第1,2个指定了用户名，命令执行后需要输入用户密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名 

​      第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名  

#### 4.2.2   复制目录

​         **scp是有Security的文件copy，基于ssh登录。 ***linux scp远程拷贝文件及文件夹,需要的朋友可以参考下*

**1.1   拷贝本机/home/administrator/test整个目录至远程主机192.168.1.100的/root目录下**， 代码如下:

```
    scp -r /home/administrator/test/ root@192.168.1.100:/root/
```



**1.2**     [root@localhost soft] # scp -r root@192.168.120.204:/opt/soft/mongodb /opt/soft/

​           从192.168.120.204机器上的/opt/soft/中下载mongodb 目录到本地的/opt/soft/目录来



**2、  拷贝单个文件至远程主机**，代码如下:  

```
    scp /home/administrator/Desktop/old/driver/test/test.txt root@192.168.1.100:/root/
```

​          其实上传文件和文件夹区别就在参数 -r， 跟cp, rm的参数使用差不多， 文加价多个 -r

###  4.2   cp

​      参数说明：

```
-a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
-d：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式。
-f：覆盖已经存在的目标文件而不给出提示。
-i：与-f选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖。
-p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
-r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
-l：不复制文件，只是生成链接文件。
```

- ​       案例1：//复制 flags.c 到flags_checkered.c 文件，当前文件同属于同一目录下      

```
   cp flags.c flags_checkered.c
```

- 
  ​       案例2：//复制 lab07文件夹下的所有文件到 lab09 文件夹下

```
   cp -r /home/user05/lab07/* /home/user05/lab09
```

- ​       案例3：//复制当前文件夹下的 flags.c 文件到 lab09 文件夹下flags_recised.c 文件

```
   cp flags.c /home/user05/lab09/flags_revised.c
```

------

## 5  同步时间

Cenros7

   ntp常用时间服务器：

` NTP服务器(上海) ：ntp.api.bz`
` 中国国家授时中心：210.72.145.44`
 `美国：time.nist.gov`
` 复旦：ntp.fudan.edu.cn`
` 微软公司授时主机(美国) ：time.windows.com`
` 台警大授时中心(台湾)：asia.pool.ntp.org`

1.   流程：

```
安装
yum install ntpdate -y

同步
ntpdate ntp1.aliyun.com
```

2.  使用ntpd server，进行集群配置。网搜索！

## 6.jdk路径问题



### 6.1  **Centos7**

在可执行 java命令的情况下查找过程如下:

- 命令：which java

```
[root@localhost ~]# which java
                   /usr/bin/java
```

- 命令：ls -lrt /usr/bin/java

```
[root@localhost ~]# ls -lrt /usr/bin/java
                   lrwxrwxrwx. 1 root root 22 10月 10 08:06 /usr/bin/java -> /etc/alternatives/java
```

- 命令：ls -lrt /etc/alternatives/java

```
[root@localhost ~]# ls -lrt /etc/alternatives/java
lrwxrwxrwx. 1 root root 73 10月 10 08:06 /etc/alternatives/java -> /usr/lib/jvm/java-1.8.0-[openjdk](https://so.csdn.net/so/search?q=openjdk&spm=1001.2101.3001.7020)-1.8.0.144-0.b01.el7_4.x86_64/jre/bin/java
```

- 由上可知java的路径为: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.144-0.b01.el7_4.x86_64,进入该路径查看文件如下:

```
[root@localhost ~]# cd /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.144-0.b01.el7_4.x86_64
[root@localhost java-1.8.0-openjdk-1.8.0.144-0.b01.el7_4.x86_64]# ll
总用量 4
drwxr-xr-x. 2 root root 4096 10月 10 14:53 bin
drwxr-xr-x. 3 root root 132 10月 10 14:53 include
drwxr-xr-x. 4 root root 28 10月 10 08:03 jre
drwxr-xr-x. 3 root root 144 10月 10 14:53 lib
drwxr-xr-x. 2 root root 204 10月 10 14:53 tapset
```



### 6.2  **unbunt 20.04**

- 使用 which [-a] filename ... 命令查找一个命令（第 1 行）：


```
$ which java
/usr/bin/java
```

- 根据上一步骤返回的信息，使用 file 命令确定文件类型（第 1 行），可知 /usr/bin/java 是一个指向 /etc/alternatives/java 的 symbolic link：


```
$ file /usr/bin/java
/usr/bin/java: symbolic link to /etc/alternatives/java
```

- 继续使用 file 命令确定文件类型（第 1 行）：

```
$ file /etc/alternatives/java
/etc/alternatives/java: symbolic link to /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
```

- 继续使用 file 命令确定文件类型（第 1 行）：


```
$ file /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java 
/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=aa219a43c6b3b442aea937fbb19cc8fb2ce3f346, for GNU/Linux 3.2.0, stripped
```

​          从以上返回的信息可知，java 命令最终是指向 /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java，由此可知 JDK 的安装目录是          

​           /usr/lib/jvm/java-8-openjdk-amd64。

## 7  主机名

centos

1、查看当前系统主机名

```text
[root@localhost ~]# hostname
localhost.localdomain
```

2、使用hostnamectl来重新设置主机名是永久生效的即使是服务器重启也生效。

```text
 hostnamectl set-hostname  主机名;
```

## 8  用户

### 8.1  **centos 7修改U&P**

- 修改用户名

1、登出要修改用户名的用户（没有注销登录的用户无法修改）
2、切换到root账号修改用户名和用户根目录。

```bash
vim /etc/passwd
# 修改用户名和用户根目录
```

找到要修改原来用户名的配置行，将这行出现的全部改（一般在最后几行）

```bash
vim /etc/shadow
# 做法同上
```

找到要修改原来用户名的配置行，将这行出现的全部改（一般在最后几行）

```
vim /etc/group
# 修改用户组，将用户组名改为新用户名
```

3、修改用户根目录名

```bash
mv /home/old  /home/new       
# 最后，修改用户根目录名
```

4、进入/home下，即可查看该设备当前的所有用户名     ls查看



- 修改用户密码

1、切换到root账号

```bash
su root
```

2、修改密码

```bash
passwd new
#passwd 用户名
```

### 8.2  **Centos7删除U

```bash
userdel -r test #删除用户和用户主目录下所有文件，不加-r删除用户文件不删除
groupdel testgroup	#删除用户组
```

### 8.3  **centos 7添加U&P**

**  一、添加用户和密码**

1.登录系统切换到root

```
[root@centos /]$ su	#切换到root用户
密码：
[root@centos /]#
```

2.添加用户

```
[root@centos /]# adduser test	#新建test用户
[root@centos /]# 
```

3.设置用户密码
  注意：不按要求的密码也是可以的

```
[root@centos /]# passwd test	#给test用户设置密码
更改用户 test 的密码 。
新的 密码：
无效的密码： 密码少于 8 个字符
重新输入新的 密码：
passwd：所有的身份验证令牌已经成功更新。
[root@centos /]#
```

  二、  **给用户添加root权限**

```
[root@centos /]# vi /etc/sudoers  #进入sudoers给test添加权限
找到以下位置，按i进行编辑：
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
test		ALL=(ALL)   ALL   	  #在root下面，添加这条即可授权（所有权限），test为用户名
按ESC输入 :wq!  保存即可 
现在用户test就有root权限
```

   测试权限是否可用：

```
使用root创建一个文件夹：
mkdir test
在切换到xxx用户，将该文件删除（发现即可删除）：
su xxx
rm -f test
```

**ubunt**

**添加用户**

1. 使用`adduser`创建用户，命令如下：

```text
adduser newuser
```

​     2.1  给新用户添加管理权限

​    如果希望新创建的用户具有管理权限，将用户添加到`sudo`组即可！

​    将新用户添加到 `sudo` 组，命令如下：

```text
adduser newuser sudo
```

​    2.1  给新用户添加管理权限

这里采用修改Ubuntu 18.04系统/etc/sudoers文件的方法分配用户权限。因为此文件只有r权限，在改动前需要增加w权限，改动后，再去掉w权限。

```
sudo chmod +w /etc/sudoers

sudo vim /etc/sudoers

# 添加下图的配置语句，并且保存修改
```

sudo chmod -w /etc/sudoers

```
# User privilege specification

root　    ALL=(ALL:ALL)  ALL
User_new  ALL=(ALL:ALL)  ALL
```



3.   账号切换

​    由`root`账号切换到普通账号：

```text
sudo su newuser
```

由普通账号切换到`root`账号：

```text
sudo su root
```

**删除用户**

如果要删除用户，请按下面操作进行，分为3步：

1、执行userdel命令：sudo userdel old

2、删除用户目录命令：sudo rm -rf /home/old

3、删除用户权限相关配置：删除或者注释掉/etc/sudoers中关于要删除用户的配置，否则无法再次创建同名用户。

## 9  文件

**文件树**

  1、 包管理器安装

centos 中用         yum -y install tree 

ubuntu 中用        apt-get install tree 

当然如果需要权限不要忘了在前面加上 sudo

  2、 源码编译安装

```
wget ftp://mama.indstate.edu/linux/tree/tree-1.6.0.tgz
tar xzvf tree-1.6.0.tgz
cd tree-1.6.0
make && make install
```

​     最后可以 ：  cp tree /bin 

**mkdir命令**

  1 . `[root@duan ~]# mkdir abc`，在当前目录创建目录abc。

  2 . `[root@duan ~]# mkdir -p /aa/bb/cc`，在/根目录创建目录/aa，在/aa目录下创建子目录bb，在/aa/bb目录下创建子目录cc。

**rm 删除命令**

  常用选项:

| 选项 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| -f   | 删除文件或目录时不进行提醒，而直接强制删除。                 |
| -i   | 删除文件或目录时提醒用户确认。( y表示删除，n表示不删除）     |
| -r   | 删除目录时必须使用此选项，表示**递归删除**整个目录树（应谨慎使用）。 |

  如：

```
rm test.php        #删除文件需要确认

rm -f test.php     #强制删除文件不确认

rm -rfv ./test     #递归强制删除当前的test目录，并显示删除的详细过程

rm -rf aa/         #递归强制删除aa目录下的所有
```

**mv命令**

 mv 移动文件或目录、对单个文件进行重命名。

 格式：`mv [选项] 源文件或目录 目标文件或目录`

- 将指定的文件或目录转移位置；
- 如果目标位置与源位置相同，则相当于**重命名**操作。.

1.  移动并且重命名：

```handlebars
mv 1.txt /root/aa/2.txt   #移动后文件名变成 2.txt
```

2.  重命名：

```
[root@master ab]# ll 
a.txt      b.txt
[root@master ab]# mv a.txt  c.txt  
[root@master ab]# ll 
c.txt      b.txt
```

**rename**命令

       使用 rename 批量修改文件名：

格式：`rename 旧字符 新字符 文件名`
格式：`rename 旧文件 新文件 目标文件`

如：`rename jpg txt *.jpg`

```
rename abc def abc    # 修改 abc 名字为 dec
```

## 10 Finalshell 问题

### 10.1 连接超时问题

1.  配置域名

   ```
   cd C:\Windows\System32\drivers\etc\hosts
   ```

2.  虚拟机网卡设置问题

   ```
   cd /etc/sysconfig/network-scripts
   
   vim ifcfg-ens33
   ```

   配置网卡：

   ```
   BOOTPROTO=static #静态ip
   TYPE=Ethernet
   IPADDR=192.168.25.120 
   GATEWAY=本机ip
   NETMASK=255.255.255.0
   ```

   重启网络服务：

   ```
   systemctl stop NetworkManager                    临时关闭
   systemctl disable NetworkManager                 永久关闭网络管理命令
   systemctl start network.service                  开启网络服务
   ```

**注意：本机能ping通虚拟机，finalshell就能连接**

3. 设置静态ip

    打开命令行，输入【vim /etc/sysconfig/network-scripts/ifcfg-ens33 】，并修改配置文件内容。

\#ip
IPADDR=192.168.200.200
NETMASK=255.255.255.0
\#gateway
GATEWAY=192.168.200.2
\#dns
DNS1=192.168.200.2 和网关 一致

[VMware虚拟机中配置静态IP_虚拟机配置静态ip-CSDN博客](https://blog.csdn.net/qq_40172610/article/details/120447600)

[VMware虚拟机网络配置-NAT篇 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/130984945)

4.关闭防火墙

### 10.2 本地文件上传失败

​    使用root用户登陆，即可在本地与finalshell之间拖拽文件

------

## 11  vim

**六种编辑模式：**

  i ：向光标前方插入信息

  I（大写i） ：在光标所在行的行首插入信息

  a ：向光标后方插入信息

  A ：在光标所在行的行末插入信息

  o ：向光标所在行的下一行插入信息

  O ：向光标所在行的上一行插入信息

**Vim命令：**

```
gg ：移动到代码顶部（第一行）
yy ：复制光标所在的行
数字+yy ：复制光标所在的行开始向下共指定数字行（例 ：3yy就是从光标开始算起向下共3行）
dd ：剪切光标所在行
数字+dd ：剪切光标所在的行开始向下共指定数字行
D ：从光标位置开始剪切，一直到行末
d0 ：从当前位置开始剪切，一直到行首
dw ：剪切一个单词
x ：剪切当前光标，按一次剪切一个
X ：剪切当前光标的前一个，按一次剪切一个
u ：撤销刚才的操作
wq ：（末行模式）保存并退出
shift + zz ：（命令模式）保存并退出
```

## 12 hadoop问题

集群搭建 [Hadoop-2.7.4 集群快速搭建-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1041236)

### 12.1 50070打不开

1. 输入`jps`检查是否正确开启服务
2. 在hdfs-site.xml中，更改开放端口的绑定IP

```
<name>dfs.http.address</name>

<value>0.0.0.0:50070</value>

</property>
```

​     将绑定IP改为0.0.0.0，而不是本地回环IP，这样，就能够实现外网访问本机的50070端口了。

​     关闭防火墙：终端输入   `systemctl stop firewalld.service` 

3. 禁止自动启动就用 `systemctl disable firewalld.service`
4. hadoop2 端口是50070 ，hadoop3 端口是    localhost:9870 

### 12.2  datanode无法启动

​      我们每次在格式化namenode时都会产生一个新的集群ID,如果格式化成功，在命令行输出信息里就有新产出的集群ID.

​      我们都应该在core-site.xml文件里配置了hadoop.tmp.dir参数，这个参数的属性值的子文件是dfs，dfs里面有name和data两个子文件夹，里面分别存放namenode和daranode节点的数据。它们各自的集群ID信息分别存放在name(data)下的current文件夹里的VERSION文件里。

​    解决办法，格式化：

1. 关闭集群
2. 清空name和data文件夹里的所有东西
3. hdfs namenode -format 格式化namenode
4. 重启即可

#### 12.3 namenode启动date未启动

[启动hadoop集群,namenode正常启动，而datanode没有启动或只启动一个，在web端只看到一个datanode的原因及其解决办法_hdfs只有一个datanode-CSDN博客](https://blog.csdn.net/m0_58028961/article/details/122985159)