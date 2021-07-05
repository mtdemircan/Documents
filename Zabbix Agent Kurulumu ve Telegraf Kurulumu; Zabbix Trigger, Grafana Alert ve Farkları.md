# Zabbix ile istemci metriklerinin izlenmesi ve trigger oluşturulması(Debian 10)

## 1-Zabbix agent kurulumu

 Bilgi almak istediğimiz bilgisayara agent kuracağız.



```
wget https://repo.zabbix.com/zabbix/4.0/debian/pool/main/z/zabbix-release/zabbix-release_4.0-3+buster_all.deb
sudo dpkg -i zabbix-release_4.0-3+buster_all.deb
```

```
sudo apt-get update
sudo apt-get install zabbix-agent
```

```
sudo nano /etc/zabbix/zabbix_agentd.conf
```

içeriğini değiştirelim

```
#Server=[zabbix server ip]
#ServerActive=[zabbix server ip]
#Hostname=[Hostname of client system ]

örnek:
Server=192.168.1.10
ServerActive=192.168.1.10
Hostname=Server2
```

```
service zabbix-agent start
```

```
sudo systemctl enable zabbix-agent
sudo systemctl start zabbix-agent
```

## 2-Zabbix web arayüzünde agentı yüklediğimiz bilgisayarı host olarak ekleme

![resim](https://tecadmin.net/wp-content/uploads/2013/10/zabbix-add-host-1.png)

Configuration --> Hosts --> Create Host

Hostname -- agent bulunan bilgisayarın hostname 

Visible name -- zabbix üzerinde görünecek isim

Groups -- select diyelim ve linux servers seçelim

Interfaces -- agentin olduğu bilgisayar IP adresi

 Eklediğimiz host a basalım Templates kısmına girelim

Link new templates --> select deyip Templates OS Linux by Zabbix agent seçelim

uptade e basalım

## 3-Eklediğimiz hostta trigger oluşturma

Configuration --> Hosts --> trigger oluşturacağımız hostu seçelim --> Triggers --> Create Trigger

Name = Trigger ismi

Expression =  Add a basın Item --> Select diyerek hangi bilgiyi istiyorsak seçelim Result kısmında ise seçtiğimiz bilgi hangi değerlerdeyse trigger çalışsın onu seçiyoruz.

Add 

Eklediğiniz trigger Trigger sayfasında gözükecektir.



# Grafana ile istemci metriklerini izleme ve alarm kurma (Debian 10)

## 1- Istemciye Telegraf kurulumu



1-**sudo apt update && sudo apt -y upgrade** 
2-**sudo apt install -y gnupg2 curl wget** 
3-**wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -** 
4-**echo "deb https://repos.influxdata.com/debian buster stable" | sudo tee /etc/apt/sources.list.d/influxdb.list** 

Depoları ekledikten sonra yüklemeye başlayabiliriz. 
5-**sudo apt update** 
6-**sudo apt -y install telegraf** 

Kontrol edelim: 
**systemctl status telegraf** 



## 2-Konfigürasyon

nano /etc/telegraf/telegraf.conf

[agent] in altında hostname='' satırında sizin serverinizin hostname ini yazın.\
[[outputs.influxdb]] nin altındaki \
urls=["http://influxdb-ip:8086"] içeriğini http://ipadresiniz:8086 ile değiştirin.\
database="database-name" içeriğini InfluxDB database isminiz ile değiştirin.\
username="auth-username" yerine InfluxDB kullanıcı adınızı\
password="auth-password" yerine de InfluxDB parolanızı yazın.

Değişikliklerden sonra sistemi yeniden başlatalım:

sudo systemctl start telegraf && systemctl enable telegraf

## 3-Grafana web arayüzünden InfluxDB data source ekleme

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

## 4-Alarm kurma

Zabbixteki triggerlara benzer şekilde burada da alertler vardır.

Bir alert oluşturmak için dashboarddan alarm kurmak istediğiniz panelin isminin yanındaki oka basın ve edit deyin. Ardından alt sekmede Alert e geçin. Create Alert:

Name=Istediğimiz bir isim.

Conditions kısmı aslında en önemli kısım. Burada hangi koşul altında alarmın çalışmasını istiyorsak onu belirliyoruz. Kaydedip çıkabiliriz. Alarmlarınızı görmek için soldaki menüdeki alarm işaretine basın.



# Trigger oluşturma açısından Zabbix ve Grafana arasındaki farklar



Zabbixte trigger dediğimiz şey aslında seçtiğimiz hosttan elde ettiğimiz itemleri mantıksal ifade olarak değerlendirir ve bize ona göre bilgi verir.

Grafanada ise alertler vardır. Alert seçilen panel üzerinde belli şartlar sağlanırsa oluşacak bir uyarıdır. Zabbixte bu kontroller hostlar üzerinde oluşturulurken Grafanada paneller üzerinde oluşturulur.

Zabbixte trigger şartı için olan logical expression tek bir yerde oluşturulurken Grafanada Query ve Alert sekmesi ayrıdır yani alacağımız bilgiyi farklı sekmede bu bilginin hangi şartları sağlayacağı da farklı bir sekmededir.





