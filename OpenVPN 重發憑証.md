#OpenVPN 重發憑証
===============
openvpn 看起來是沒有重發憑證的相關指令,不過是可以手動修改建立。一般初始建立完openvpn憑證後,會在 /etc/openvpn/2.0/keys/index.txt 建立一筆 此憑證紀錄,並在/etc/openvpn/2.0/keys/serial 序號 加 1,因此知道過程之後,就可以用手動方式調整/etc/openvpn/2.0/keys/{index.txt,serial} ,來達到重發憑證要求。

###檢查憑證是否過期 
(這邊用splus DC 來當作例子)

###開始重發憑証
