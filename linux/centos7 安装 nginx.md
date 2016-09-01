#centos安装nginx
## 1. centos安装连接
1. parallels 桥接默认适配器即可
2. **ifconfig 命令更新为 ip addr** (yum install net-tools 可恢复使用 ifconfig)
3. securecrt 连接centos 使用root用户
## 2. nginx安装
### 2.1环境
1. gcc
*yum install gcc-c++*
2. PCRE
*yum install -y pcre pcre-devel*
3. zlib
*yum install -y zlib zlib-devel*
4. openssl
*yum install -y openssl openssl-devel*
###2.2 安装
1. 将nginx-1.8.0.tar.gz拷贝至linux服务器。
2. 命令
```
//解压
tar -zxvf nginx-1.8.0.tar.gz
cd nginx-1.8.0 
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi

注意：上边将临时文件目录指定为/var/temp/nginx，需要在/var下创建temp及nginx目录
[root@bogon nginx-1.8.0]# mkdir /var/temp/nginx -p
// 编译安装
make
make  install
```
## 3 启动nginx
```
cd /usr/local/nginx/sbin/
./nginx 
```
注意：执行./nginx启动nginx，这里可以-c指定加载的nginx配置文件，如下：
./nginx -c /usr/local/nginx/conf/nginx.conf
如果不指定-c，nginx在启动时默认加载conf/nginx.conf文件，此文件的地址也可以在编译安装nginx时指定./configure的参数（--conf-path= 指向配置文件（nginx.conf）） 








