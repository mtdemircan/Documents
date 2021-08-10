# Samba Kullanıcı İşlemleri Logları

Sambanın kurulu olduğu sunucu üzerinde  **smb.conf** dosyasını açalım.

`nano /etc/samba/smb.conf` 

[global] kısmının altına aşağıdaki satırı ekleyelim.

`log level = 1 dsdb_audit:5@/var/log/dsdb_audit.log`

Servisi yeniden başlatalım.

`systemctl restart samba.service`

logları görmek için:

`tail -f /var/log/dsdb_audit.log`

Kullanıcı eklendiğinde oluşan log:

`[2021/08/03 19:05:18.983910,  5] ../../lib/audit_logging/audit_logging.c:95(audit_log_human_text)
  DSDB Change [Add] at [Tue, 03 Aug 2021 19:05:18.983867 +03] status [Success] remote host [ipv4:192.168.1.25:54231] SID [S-1-5-21-1379052523-3373471775-2990675676-500] DN [cn=deneme2,CN=Users,DC=rdomain,DC=lab] attributes [cn [deneme2] objectclass [top] [person] [organizationalPerson] [user] sAMAccountName [deneme2] sn [deneme2] userAccountControl [546] userPrincipalName [deneme2@rdomain.lab]]`



Kullanıcı silindiğinde oluşan log:

`[2021/08/03 19:20:03.011699,  5] ../../lib/audit_logging/audit_logging.c:95(audit_log_human_text)
  DSDB Change [Delete] at [Tue, 03 Aug 2021 19:20:03.011680 +03] status [Success] remote host [ipv4:192.168.1.25:54231] SID [S-1-5-21-1379052523-3373471775-2990675676-500] DN [cn=deneme2,CN=Users,DC=rdomain,DC=lab]`







