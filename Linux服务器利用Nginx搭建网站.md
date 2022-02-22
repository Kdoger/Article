需要注意的事情：
1. 已经申请了云服务器
2. 已经注册域名，做了域名解析，本文的域名为 www.kdog.top
3. Nginx安装目录：/etc/nginx/

---
## 利用Nginx搭建网站（Ubuntu系统）
### 步骤1：安装Nginx
1.执行以下命令，安装Nginx

```
sudo apt-get install nginx
```
2.执行以下命令，查看Nginx服务状态

```
sudo systemctl status nginx     # 出现active(running)则表示成功
```
3.执行以下命令，更改防火墙状态

```
sudo ufw allow 'Nginx Full'
```
4.在本地浏览器地址栏中输入云服务器公网IP或者域名，会出现Nginx的页面，则安装成功

### 步骤2：设置Nginx
1.执行以下命令，创建目录结构

```
sudo mkdir -p /var/www/www.kdog.top/src/
```
2.进入上述所建目录下，创建index.html页面

```
sudo vim index.html   # 
```

```
# 输入一下代码
<?xml version="1.0" encoding="UTF-8"?>
  
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">

<head>

       <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />

       <title> index</title>

</head>

<body>

<h1>hello world!!!</h1>
web_path: /var/www/www.kdog.top/src/index.html
</body>

</html>
```
### 步骤3: 创建服务器块
1.执行以下命令，创建基本配置文件

```
cd /etc/nginx/sites-available
sudo nano www.kdog.top   # 以域名为文件名创建
```

```
# 输入一下内容
server {
    listen 80;
    listen [::]:80;
    root /var/www/www.kdog.top/src/;
    index index.html;
    server_name www.kdog.top;
    access_log /var/log/nginx/www.kdog.top.access.log;
    error_log /var/log/nginx/www.kdog.top.error.log;
    location / {
        try_files $uri $uri/ =404;
    }
}
```
2.执行以下代码，启用新的服务器块文件

```
sudo ln -s /etc/nginx/sites-available/www.kdog.top /etc/nginx/sites-enable/
```
3.执行以下代码，查看nginx是否正确

```
sudo nginx -t
```
4.执行以下代码，重启nginx服务

```
sudo systemctl restart nginx
```

### 步骤4: 测试
在本地浏览器输入IP地址或域名打开网站

### tips：
1.如果出现错误，可能是配置文件中 root 关键字后面的路径出现错误，或该路径下的 html 文件命名有错误或不存在；也可能是权限问题，修改 /etc/nginx/nginx.conf 文件中 user 关键字后的用户名，改为 root 用户。

2.报错日志可在 /var/log/nginx/xxxx.error.log 中查看

##### 参考文章：
1.https://blog.csdn.net/weixin_45406882/article/details/107071741

2.https://blog.csdn.net/weixin_29003023/article/details/115489427
