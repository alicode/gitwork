#OpenVPN 重發憑証

openvpn 看起來是沒有重發憑證的相關指令,不過是可以手動修改建立。一般初始建立完openvpn憑證後,會在 /etc/openvpn/2.0/keys/index.txt 建立一筆

此憑證紀錄,並在/etc/openvpn/2.0/keys/serial 序號 加 1,因此知道過程之後,就可以用手動方式調整/etc/openvpn/2.0/keys/{index.txt,serial}

,來達到重發憑證要求。

###檢查憑證是否過期 
(這邊用splus DC 來當作例子)
1.按照經驗,server 憑證(ca.crt , )及client憑證都檢查看看是否過期。(notAfter那一行為過期日)
2.Server端憑證
(ca.crt)
```sh
echo "$(date +%Y-%m-%d)";openssl x509 -noout  -dates -in /etc/openvpn/2.0/keys/ca.crt
```
(splus.crt)
```sh
echo "$(date +%Y-%m-%d)";openssl x509 -noout  -dates -in /etc/openvpn/2.0/keys/splus.crt
```
3.Client端憑證
(SplusOP.crt)
```sh
echo "$(date +%Y-%m-%d)";openssl x509 -noout  -dates -in /etc/openvpn/2.0/keys/SplusOP.crt
```
###開始重發憑証


* (如何看憑證日期)[http://www.shellhacks.com/en/HowTo-Check-SSL-Certificate-Expiration-Date-from-the-Linux-Shell]
