
###1.下载mysql5.7

官网下载mysql-5.7.17-linux-glibc2.5-x86_64.tar.gz，下载地址为：
https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.17-linux-glibc2.5-x86_64.tar.gz

###2.文件上传centos并解压为文件
```
本地解压为mysql-5.7.17-linux-glibc2.5-x86_64.tar  
上传服务器centos
tar xvf mysql-5.7.11-linux-glibc2.5-x86_64.tar
```
###3.添加用户mysql用户等
```
//添加mysql用户组
groupadd mysql  

useradd -r -g mysql -s /bin/nologin mysql   // -s /bin/nologin 表示mysql 不能作为登入用户

cd /usr/local/mysql/
mkdir data   //创建数据库保存文件夹

chmod 770 data
sudo chown -R mysql .   //注意最后有个点(改变所有文件)

sudo chgrp -R mysql .
```
###4.初始化mysql
```
//初始化mysql
bin/mysqld --initialize  --user=mysql  --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data

[Note] A temporary password is generated for root@localhost: !x?df8oj6f8   //注意红色部分，

一定要记下来，这是生成的root用户的密码，待会登陆需要


bin/mysql_ssl_rsa_setup --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data 

chown -R root .
chown -R mysql data

bin/mysqld_safe --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data

//将mysql加入到启动服务文件夹内，就可以用 service mysql start | stop |restart 控制启动，关闭及重启mysql 
cp support-files/mysql.server /etc/init.d/mysqld 
```
###5.配置my.cnf配置文件

```
# vim /etc/my.cnf

[mysqld]
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
socket=/usr/local/mysql/data/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

[mysqld_safe]
#log-error=/var/log/mysqld.log
#pid-file=/var/run/mysqld/mysqld.pid
log-error=/usr/local/mysql/data/mysql.log
pid-file=/usr/local/mysql/data/mysql.pid
[client]
socket=/usr/local/mysql/data/mysql.sock

```
###6.配置环境变量
```
vim /etc/profile

//在文件的最后添加

export MYSQL_HOME="/usr/local/mysql"
export PATH="$PATH:$MYSQL_HOME/bin"

source /etc/profile  //使环境变量生效


//启动mysql服务
# service mysqld start
```
