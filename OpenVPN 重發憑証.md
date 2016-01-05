#OpenVPN 重發憑証

openvpn 看起來是沒有重發憑證的相關指令,不過是可以手動修改建立。一般初始建立完openvpn憑證後,會在 /etc/openvpn/2.0/keys/index.txt 建立一筆
此憑證紀錄,並在/etc/openvpn/2.0/keys/serial 序號 加 1,因此知道過程之後,就可以用手動方式調整/etc/openvpn/2.0/keys/{index.txt,serial}
,來達到重發憑證要求。

###檢查憑證是否過期  (這邊用splus DC 來當作例子)
1. 按照經驗,server 憑證(ca.crt , )及client憑證都檢查看看是否過期。(notAfter那一行為過期日)
2. Server端憑證

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
1. 到這個目錄 /etc/openvpn/easy-rsa/2.0/keys2
2. 要修改 serial index.txt這兩個檔案
3. 將要重發憑證 例如 op憑證  在 index.txt 內容哪一行刪掉,並且op 的id 是多少記下來 (例如 id=07)
4. 先看目前 serial檔案紀錄 id 是多少(例如 id=09),再將換成 id=07
5. 這樣就可以重新發OP憑證 
```sh
source ./vars     #注意過程中會提示,執行./clean-all 。請不要執行此script ,因為會把現有的 /etc/openvpn/2.0/keys/都刪除
./build-key op 
```
若server 及 ca 憑證過期,也是仿照上述1-5步驟執行。ca 憑證過期,所有Client 和 server 憑證都需要重新簽發。
```sh
./build-key-server splus
./build-ca
```
另外,**server 和 ca 憑證 重新簽發後,OpenVPN 需要 restart 或 reload 設定檔**
```sh
/etc/init.d/openvpn  reload   
 或是
 /etc/init.d.openvpn restart
```




###參考資料
* [如何看憑證日期](http://www.shellhacks.com/en/HowTo-Check-SSL-Certificate-Expiration-Date-from-the-Linux-Shell)
