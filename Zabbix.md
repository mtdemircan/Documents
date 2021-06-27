# Zabbix Kurulumu

Zabbix, Alexei Vladishev tarafından geliştirilen ağlar ve uygulamalar için bir kurumsal açık kaynak izleme çözümüdür.
Çeşitli ağ hizmetleri, sunucular ve diğer ağ donanımlarını izlemek ve durumunu takip etmek için tasarlanmıştır. 
Zabbix MySQL, PostgreSQL, SQLite, Oracle veya IBM DB2 kullanarak veriyi saklar.

1-**wget --no-check-certificate https://repo.zabbix.com/zabbix/5.0/debian/pool/main/z/zabbix-release/zabbix-release_5.0-1+buster_all.deb** \
2-**dpkg -i zabbix-release_5.0-1+buster_all.deb** \
3-**apt update**

4-**apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent**

4.1-**sudo apt-get install mariadb-server**\
5-**mysql -uroot -p**\
Şifrenizi girin

6-**create database zabbix character set utf8 collate utf8_bin;**\
7-**create user zabbix@localhost identified by 'şifrenizigirin';**\
8-grant all privileges on zabbix.* to zabbix@localhost;\
9- **quit;**



10-zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix\
11- **nano /etc/zabbix/zabbix_server.conf**\
DBPassword=password\
12- nano /etc/zabbix/apache.conf\
php_value date.timezone Europe/Riga   başındaki numara işaretini(#) kaldırın    


**systemctl restart zabbix-server zabbix-agent apache2**\
**systemctl enable zabbix-server zabbix-agent apache2**


http://sunucu ip adresiniz/zabbix  adresine gidin.

next step->next step \
parolanızı girip next step deyin \
finish dedikten sonra çıkan giriş ekranında\
Kullanıcı adı : Admin \
Şifre : zabbix \

Administration->Users
Admine basın
 ve şifrenizi değiştirin 


