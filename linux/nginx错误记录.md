1. **[error] open() "/var/run/nginx/nginx.pid" failed (2: No such file or directory)**

```

[root@localhost sbin]# ./nginx -s reload

nginx: [error] open() "/var/run/nginx/nginx.pid" failed (2: No such file or directory)


　　// 解决方法：
　　[root@localhost nginx]# /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
　　使用nginx -c的参数指定nginx.conf文件的位置
　　[root@localhost nginx]# cd logs/
　　[root@localhost logs]# ll
```

2. 403 Forbidden

 **权限不够，www目录下没index.html。**

（权限 配置文件头加上 user root;）

