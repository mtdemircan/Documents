# Samba Kullanıcı İşlemleri Logları

Sambanın kurulu olduğu sunucu üzerinde  **smb.conf** dosyasını açalım.

`nano /etc/samba/smb.conf` 

[global] kısmının altına aşağıdaki satırı ekleyelim.

`log level = 1 dsdb_json_audit:5@/var/log/dsdb_json_audit.log`

Servisi yeniden başlatalım.

`systemctl restart samba.service`

logları görmek için:

`tail -f /var/log/dsdb_json_audit.log`

Kullanıcı eklendiğinde oluşan log:

`{"timestamp": "2021-08-11T08:37:19.385124+0300", "type": "dsdbChange", "dsdbChange": {"version": {"major": 1, "minor": 0}, "statusCode": 0, "status": "Success", "operation": "Add", "remoteAddress": "ipv4:192.168.1.25:52556", "performedAsSystem": false, "userSid": "S-1-5-21-1379052523-3373471775-2990675676-500", "dn": "cn=deneme2,CN=Users,DC=rdomain,DC=lab", "transactionId": "891ebdae-db3e-4a6d-9375-93838890b3e9", "sessionId": "98b403a8-4c9f-4f8c-848d-0933ddbbf6d3", "attributes": {"cn": {"actions": [{"action": "add", "values": [{"value": "deneme2"}]}]}, "objectclass": {"actions": [{"action": "add", "values": [{"value": "top"}, {"value": "person"}, {"value": "organizationalPerson"}, {"value": "user"}]}]}, "sAMAccountName": {"actions": [{"action": "add", "values": [{"value": "deneme2"}]}]}, "sn": {"actions": [{"action": "add", "values": [{"value": "deneme2"}]}]}, "userAccountControl": {"actions": [{"action": "add", "values": [{"value": "546"}]}]}, "userPrincipalName": {"actions": [{"action": "add", "values": [{"value": "deneme2@rdomain.lab"}]}]}}}}`



Kullanıcı silindiğinde oluşan log:

`{"timestamp": "2021-08-11T08:37:30.054475+0300", "type": "dsdbChange", "dsdbChange": {"version": {"major": 1, "minor": 0}, "statusCode": 0, "status": "Success", "operation": "Delete", "remoteAddress": "ipv4:192.168.1.25:52556", "performedAsSystem": false, "userSid": "S-1-5-21-1379052523-3373471775-2990675676-500", "dn": "cn=deneme2,CN=Users,DC=rdomain,DC=lab", "transactionId": "d26ab279-337e-4760-b3a9-73fea5d942b1", "sessionId": "98b403a8-4c9f-4f8c-848d-0933ddbbf6d3"}}`







