# Samba Kullanıcı İşlemleri Logları

Sambanın kurulu olduğu sunucu üzerinde  **smb.conf** dosyasını açalım.

`nano /etc/samba/smb.conf` 

[global] kısmının altına aşağıdaki satırı ekleyelim.

`log level = 1 dsdb_audit:5@/var/log/dsdb_audit.log`

Servisi yeniden başlatalım.

`systemctl restart samba.service`

logları görmek için:

`tail -f /var/log/dsdb_audit.log`





