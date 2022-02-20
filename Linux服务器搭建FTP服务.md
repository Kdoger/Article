本文搭建FTP服务的环境如下：
- Linux操作系统：Ubuntu 20.04 LTS

---

### 操作步骤
#### 步骤1：登录Linux云服务器
方式任选
### 步骤2：安装vsftpd
1.执行以下命令，安装vsftpd

```
sudo apt install -y vsftpd
```
2.执行以下命令，设置vsftpd开机自启动

```
sudo systemctl enable vsftpd
```
3.执行以下命令，启动FTP服务

```
sudo systemctl start vsftpd
```
4.执行以下命令，确认服务是否启动

```
sudo netstat -antup | grep ftp
```
### 步骤3：配置vsftpd
1.执行以下命令，创建FTP服务使用的文件目录，本文以/home/ubuntu/ftp为例，即在用户主目录下创建的ftp文件夹

```
sudo mkdir ftp
```
2.执行以下命令修改目录权限，ubuntu是Linux用户

```
sudo chown -R ubuntu:ubuntu /home/ubuntu/ftp
```
3.执行以下命令，打开vsftpd.conf文件

```
cd /etc
sudo vim vsftpd.conf
```
4.按 i 切换到编辑模式，修改配置文件vsftpd.conf
1. 在配置文件中找到并修改以下配置参数，设置匿名用户和本地用户的登录权限，设置指定例外用户列表文件的路径，并开启监听IPv4 sockets
```
anonymous_enable=NO
local_enable=YES
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd.chroot_list
listen=YES
#write_enable=YES   将前面的 # 删除，解除注释

```
2. 在行首添加 #，注释掉 listen_ipv6=YES 配置参数，关闭监听 IPv6 sockets

```
# listen_ipv6=YES
```

3. 添加以下配置参数，开启被动模式，设置本地用户登录后所在目录，以及云服务器建立数据传输可使用的端口范围值

```
local_root=/home/ubuntu/ftp
allow_writeable_chroot=YES
pasv_enable=YES
pasv_address=xxx.xx.xxx.xx #请修改为您的轻量应用服务器公网 IP
pasv_min_port=40000
pasv_max_port=45000
```
5.按 Esc 后输入 :wq 保存并退出

6.执行以下命令，在 /etc 目录下创建并编辑 vsftpd.chroot_list 文件

```
sudo vim vsftpd.chroot_list
```
7.按 i 进入编辑模式，输入用户名，一个用户名占据一行 设置完成后按 Esc 并输入 :wq 保存后退出

8.执行以下命令，重启 FTP 服务

```
sudo systemctl restart vsftpd
```
### 步骤4：验证FTP服务
利用Terminus或者FileZilla等工具验证FTP服务
