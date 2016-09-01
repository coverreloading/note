```
yum -y install vsftpd 
useradd ftpuser 
// 登录后默认的路径为 /home/ftpuser. 
passwd ftpuser 
// 安装Vim
yum -y install vim-enhanced
//   防火墙开启21端口
vim /etc/sysconfig/iptables 
// 在行上面有22 -j ACCEPT 下面另起一行输入跟那行差不多的，只是把22换成21，然后：wq保存 
// 重启iptables
service iptables restart
// 外网是可以访问上去了，可是发现没法返回目录（使用ftp的主动模式，被动模式还是无法访问），也上传不了，因为selinux作怪了。
// 修改selinux：
// 执行以下命令查看状态：
getsebool -a | grep ftp  
```
allow_ftpd_anon_write --> off
**allow_ftpd_full_access --> off**
allow_ftpd_use_cifs --> off
allow_ftpd_use_nfs --> off
**ftp_home_dir --> off**
ftpd_connect_db --> off
ftpd_use_passive_mode --> off
httpd_enable_ftp_server --> off
tftp_anon_write --> off
```
// 执行上面命令，再返回的结果看到两行都是off，代表，没有开启外网的访问
setsebool -P allow_ftpd_full_access on
setsebool -P ftp_home_dir on

```
1. 这样应该没问题了（如果，还是不行，看看是不是用了ftp客户端工具用了passive模式访问了，如提示Entering Passive mode，就代表是passive模式，默认是不行的，因为ftp passive模式被iptables挡住了，下面会讲怎么开启，如果懒得开的话，就看看你客户端ftp是否有port模式的选项，或者把passive模式的选项去掉。如果客户端还是不行，看看客户端上的主机的电脑是否开了防火墙，关吧）:主动、被动模式修改

2. 关闭匿名访问:
修改文件：/etc/vsftpd/vsftpd.conf
`anonymous_enable = No`

`service vsftpd restart`

3. 开启被动模式
默认是开启的，但是要指定一个端口范围，打开**vsftpd.conf**文件，在后面加上

```
pasv_min_port=30000
pasv_max_port=30999
```
1. 表示端口范围为30000~30999，这个可以随意改。改完重启一下vsftpd
2. 由于指定这段端口范围，iptables也要相应的开启这个范围，所以像上面那样打开iptables文件。
也是在21上下面另起一行，更那行差不多，只是把21 改为30000:30999,然后:wq保存，重启下iptables。这样就搞定了。


4. 开机自启 `chkconfig vsftpd on`
配置路径

