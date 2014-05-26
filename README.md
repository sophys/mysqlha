mysqlha
=======

  本测试采用的是32位环境，mysql-cluster版本位：mysql-cluster-gpl-7.2.7-linux2.6-i686.tar.gz，可自行去网上搜索下载。64位的话用mysql-cluster-gpl-7.2.7-linux2.6-x86_64.tar.gz。
  
  本测试mysql安装路径为/usr/local/mysql，my.cnf路径位/etc,config.ini路径位/var/lib/mysql-cluster/,默认登陆用户为root。
  下面是集群安装步骤：
  
集群的安装
======


1. 卸载原有数据库 
  apt-get -y remove mysql-server --purge
  rm -rf /etc/mysql
  rm -rf /usr/local/mysql


2. 建立相关目录
  mkdir -p /usr/local/mysql
  tar -xzvf ./mysql-cluster-gpl-7.2.7-linux2.6-x86_64.tar.gz 
  mv ./mysql-cluster-gpl-7.2.7-linux2.6-x86_64/* /usr/local/mysql


3. 安装包依赖
  apt-get -y install libaio1


4. 安装集群
/usr/local/mysql/scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data/


5. 设置集群配置文件
  mkdir -p /var/lib/mysql-cluster
  cp ./config.ini /var/lib/mysql-cluster/
  cp ./my.cnf /etc/

配置集群
=======

1. 修改密码 /usr/local/mysql/bin/mysqladmin -u root password '654321'


2. 远程访问权限
  /usr/local/mysql/bin/mysql -u root -p
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '654321' WITH GRANT OPTION;


集群启动
======

首次启动的话，需要自己手动启动

1. 管理节点
    /usr/local/mysql/bin/ndb_mgmd -f /var/lib/mysql-cluster/config.ini --initial
2. 数据节点
    /usr/local/mysql/bin/ndbd --initial
3. 访问节点
    /usr/local/mysql/bin/mysqld_safe --user=root&

mysqlhad的启动（HA的daemon进程）
========

mysqlhad 侦听55555端口
配置文件:/var/mysqlhad.conf 内容为：
···
nodeip
gateway
sleeptime
···
日志文件:/var/mysqlhadlog

mysqlhad可以放任意目录运行,但是需要以root身份运行.直接运行会创建后台daemon进程,加 -t 参数可以打印运行的一些输出信息,但是会在前台一直运行.

> 说明：只有第一次启动集群的时候需要手动启动，以后的话只启动mysqlhad程序即可，这个程序会根据实际情况来自主决定集群的启动方式。



