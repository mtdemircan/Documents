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





​	