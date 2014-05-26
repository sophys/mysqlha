mysqlha
=======
  本测试采用的是32位环境，mysql-cluster版本位：mysql-cluster-gpl-7.2.7-linux2.6-i686.tar.gz，可自行去网上搜索下载。64位的话用mysql-cluster-gpl-7.2.7-linux2.6-x86_64.tar.gz。
  本测试mysql安装路径为/usr/local/mysql，my.cnf路径位/etc,config.ini路径位/var/lib/mysql-cluster/,默认登陆用户为root。
  下面是集群安装步骤：
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


