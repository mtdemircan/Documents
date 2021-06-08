# Telegraf, InfluxDB ve Grafana Kurulumu(Debian 10 Buster)
*Mehmet Taha Demircan  7/6/2021*


## Telegraf Kurulumu 

Telegraf InfluxData tarafından geliştirilen bir izleme ajanıdır.
Go dilinde yazılmıştır ve sistem performans metriklerini ölçmek için kullanılır.

**sudo su** yazıp entera basalım. 

1-**sudo apt update && sudo apt -y upgrade** \
2-**sudo apt install -y gnupg2 curl wget** \
3-**wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -** \
4-**echo "deb https://repos.influxdata.com/debian buster stable" | sudo tee /etc/apt/sources.list.d/influxdb.list** 

Depoları ekledikten sonra yüklemeye başlayabiliriz. \
5-**sudo apt update** \
6-**sudo apt -y install telegraf** 

Kontrol edelim: \
**systemctl status telegraf** 



## InfluxDB Kurulumu

Gerçek zamanlı analitik çalışmaları, uygulama metriklerinin analizi ve saklanması gibi 
konularda kullanılan açık kaynaklı bir veritabanı türüdür.Go dilinde yazılmıştır.


Depoları ekleyelim \
1-**sudo apt update** \
2-**sudo apt install -y gnupg2 curl wget** \
3-**wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -** \
4-**echo "deb https://repos.influxdata.com/debian buster stable" | sudo tee /etc/apt/sources.list.d/influxdb.list** 

Depoları ekledikten sonra yüklemeyi yapalım\
5-**sudo apt update**\
6-**sudo apt install -y influxdb**

Sistem başladığında InfluxDB nin de başlamasını istiyorsak :\
**sudo systemctl enable --now influxdb**

Kontrol edelim:\
**systemctl status influxdb**

7-Port izinlerini ayarlayalım:\
**sudo apt -y install ufw**\
**sudo ufw enable**\
**sudo ufw allow 22/tcp**\
**sudo ufw allow 8086/tcp**

8-Dosya içeriğinde bu değişikliği yapalım
**sudo nano /etc/influxdb/influxdb.conf ** \
[http]\
 auth-enabled = true
 
9-Yeni bir kullanıcı oluşturalım. \
username yerine kullanıcı adınızı,\
password yerine parolanızı yazın.\
**curl -XPOST "http://localhost:8086/query" --data-urlencode "q=CREATE USER username WITH PASSWORD 'strongpassword' WITH ALL PRIVILEGES"**\
**influx -username 'username' -password 'password'**\
**curl -G http://localhost:8086/query -u username:password --data-urlencode "q=SHOW DATABASES"**


## Grafana Kurulumu

Açık kaynak kodlu, web uygulaması olarak çalışan bir grafik uygulamasıdır.
Farklı veri kaynaklarından aldığı verileri dashboard halinde gösterir.


1-**curl https://packages.grafana.com/gpg.key | sudo apt-key add -**\
2-**sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"**

3-**sudo apt-get update**\
4-**sudo apt-get -y install grafana**

5-Konrol edelim:\
**sudo systemctl start grafana-server**\
**systemctl status grafana-server**\
**sudo systemctl enable grafana-server**

6-Port iznini verelim:\
**sudo ufw allow proto tcp from any to any port 3000**

Servis başladıktan sonra http://ipadresiniz:3000 adresinden Grafana arayüzüne ulaşabilirsiniz.\
Bu adrese gittiğinizde kullanıcı adı ve parola soracaktır.\
kullanıcı adı:admin\
parola:admin yazın ve log in e basın.\
Parola değişim ekranında yeni parolanızı oluşturun.

## Linux Sisteminizi Grafana ve Telegraf ile Izleme

Telegraf metrikleri InfluxDB de saklanır.
Grafanayı kullanarak bu verileri görselleştireceğiz.

Dosya içeriğini değiştirelim.\
**sudo nano /etc/telegraf/telegraf.conf**\
[agent] in altında hostname='' satırında sizin serverinizin hostname ini yazın.\
[[outputs.influxdb]] nin altındaki \
urls=["http://influxdb-ip:8086"] içeriğini http://ipadresiniz:8086 ile değiştirin.\
database="database-name" içeriğini InfluxDB database isminiz ile değiştirin.\
username="auth-username" yerine InfluxDB kullanıcı adınızı\
password="auth-password" yerine de InfluxDB parolanızı yazın.

Değişikliklerden sonra sistemi yeniden başlatalım:
**sudo systemctl start telegraf && systemctl enable telegraf**

### Grafana üzerine InfluxDB data source ekleme

1-Grafanaya giriş yapın.
2-Configuration>Data Sources>Add data source.
Doldurulacak alanlar:
Name-istediğiniz bir isim
Type-InfluxDB
HTTP URL: http://serveripadresiniz:8086

InfluxDB Details altındaki alanlar:
Database ismi kısmına telegraf konfigürasyon dosyasındaki ismi yazın.
user ve password kısmına telegraf kullanıcı adı ve parolanızı girin.
HTTP method:GET
Save and Test e basalım.

Grafanada soldaki menüden + ya basıp import a basalım.
Import dashboard from file or Grafana.com yazan kısma 5955 yazın ve devam edin.

Bir isim verin ve data source olarak InfluxDB yi seçin sonra import a basın.,
Sistem metriklerini görüyor olmalısınız.




