***

![zabbix_course](https://user-images.githubusercontent.com/87377798/211203255-d19edcb1-f6cf-42db-beda-9dfbaf63538a.png)

# การติดตั้ง การใช้งาน และการอ่านผลของเครื่องมือ Zabbix

***
## ขั้นตอนที่ 1 - Create Ct สำหรับเตรียมติดตั้ง Server ของ Zabbix
## ขั้นตอนที่ 2 - Setup และ Config Zabbix
## ขั้นตอนที่ 3 - Implement Zabbix โดนใช้ SNMP 
## ขั้นตอนที่ 5 - Monitor อุปกรณ์
## ขั้นตอนที่ 6 - การแจ้งเตือน (ผ่าน Outlook365) 
 
***

## ขั้นตอนที่ 1 - Create Ct สำหรับเตรียมติดตั้ง Server ของ Zabbix

## กำหนดชื่อ กำหนดรหัสผ่าน และSSH KEY 

![323349242_3367566253515291_5797434817281545862_n](https://user-images.githubusercontent.com/87377798/211203719-2ae05f21-bd9e-4a69-baf5-f57691d327ea.png)

## เลือก os ที่ต้องการ
 
![318014612_1378125582943631_2714223770837493793_n](https://user-images.githubusercontent.com/87377798/211204548-b63faece-9c4a-4e44-979d-07ad4cfb844b.png)

## กำหนดขนาดของ Storage ของ Server

![318339087_967194557599547_4498425463763641514_n](https://user-images.githubusercontent.com/87377798/211204592-fdeeba36-8708-4637-9da7-c7bbdeb8de33.png)

## กำหนดจำนวน cpu ของ Server

![323583180_825957568482783_2351984305087874723_n](https://user-images.githubusercontent.com/87377798/211204625-b46a95ae-0d5e-4d4b-9e9d-4203ebf1d9ed.png)

## กำหนดจำนวน RAM และ SWAP

![319030589_884204209379943_1826564098299007394_n](https://user-images.githubusercontent.com/87377798/211204690-5e2e8b57-a388-4fd7-8690-aed7099ba1ae.png)

## กำหนด address ipv4 

![320148166_836777634097010_8849781503976518867_n](https://user-images.githubusercontent.com/87377798/211204720-91cba0dc-971f-4458-ae1c-9932a58b07bb.png)

## กำหนด DNS 

![321966742_675207904077387_8678904671529243397_n](https://user-images.githubusercontent.com/87377798/211204860-435ef51a-0ae8-40a1-a67a-bb0cfd25fb9f.png)

***

## ขั้นตอนที่ 2 - Setup และ Config Zabbix

***

## 1.Install Zabbix repository

```
wget https://repo.zabbix.com/zabbix/6.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.2-4%2Bubuntu22.04_all.deb
dpkg -i zabbix-release_6.2-4+ubuntu22.04_all.deb
```

![317835043_1298677780982128_2291772008469290255_n](https://user-images.githubusercontent.com/87377798/211205010-ae5a81f0-2c81-497a-a7da-a9020a93748e.png)

***

```
apt update
```

![319120873_5790704187685595_7086940751827624812_n](https://user-images.githubusercontent.com/87377798/211205109-8af716a9-2e7b-4959-baee-a0fae099756d.png)

***

## 2.Install Zabbix server, frontend, agent

```
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

## Create initial database

```
mysql -uroot -p
```
```
create database zabbix character set utf8mb4 collate utf8mb4_bin;
```
```
create user zabbix@localhost identified by 'password';
```
```
grant all privileges on zabbix.* to zabbix@localhost;
```
```
set global log_bin_trust_function_creators = 1;
```
```
quit;
```

```
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```
 ปิด log_bin_trust_function_creators option หลังจากนำข้อมูลเข้า database
 
```
mysql -uroot -p
```
## ใส่ password
```
set global log_bin_trust_function_creators = 0;
```
```
quit;
```

***

## Configure the database for Zabbix server

```
nano /etc/zabbix/zabbix_server.conf
```

## แก้ไข DBPassword=password

***

## เปิดการใช้งาน Server Zabbix

```
 systemctl restart zabbix-server zabbix-agent apache2
```
```
 systemctl enable zabbix-server zabbix-agent apache2
```

## ตั้งค่าใน Web Zabbix

## ตั้งค่าภาษา 
![322530354_844190266871842_7001748652109754554_n](https://user-images.githubusercontent.com/87377798/211206257-a7f66da7-eb50-4a67-9683-eb3cee6ab28d.png)

## เช็ค status ของ php

![318575298_1997417867135496_5747201532213256981_n](https://user-images.githubusercontent.com/87377798/211206295-14976b52-6723-43b6-a04b-9613d84483b5.png)

## ตั้งค่า database 

![319910391_723861215966851_1034635680952936012_n](https://user-images.githubusercontent.com/87377798/211206300-cb0900d8-a366-4d04-9df4-5016c56d3689.png)

## ตั้งค่า Timezone และ theme
![319824977_683672683433656_3733177406443467590_n](https://user-images.githubusercontent.com/87377798/211206351-49aeaf2a-7e22-43fa-8cbb-e667006a0714.png)

## เสร็จสิ้นการตั้งค่า

![317318760_647352243851877_7964469790553264821_n](https://user-images.githubusercontent.com/87377798/211206410-96b2bc81-0dea-4b2e-b2ca-718094fcc3bc.png)

![318945220_601296421998007_126078065550588884_n](https://user-images.githubusercontent.com/87377798/211206429-3e06b162-a66a-4d9c-a3f9-1588f5b3cd69.png)

## เวอร์ชั่นล่าสุดต้องทำการแก้ไข ติดตั้งภาษา local ในเครื่อง Server

![322822677_1296825954213185_4717355358310065091_n](https://user-images.githubusercontent.com/87377798/211206440-ed124418-7759-4935-a1b8-33a3d1e02a96.png)

![318581762_888040522548276_4411690178153015576_n](https://user-images.githubusercontent.com/87377798/211206478-11ea2e34-46b7-41bc-9819-dc8e57756753.png)

## แก้ไขเสร็จสิ้น

![319326173_975429466907173_3869390881423672163_n](https://user-images.githubusercontent.com/87377798/211206493-7f1efc69-263c-4919-8c3e-726efdab92ca.png)

***

## 3.Implement Zabbix โดนใช้ SNMP

```
apt-get update
```
```
apt-get install snmpd snmp
```
```
nano /etc/snmp/snmpd.conf
```
## เพิ่มและแก้ไขคำสั่งในไฟล์ snmpd.conf

```
agentAddress udp:กำหนดip:161 
view systemonly included .1.3.6.1.2.1.1
view systemonly included .1.3.6.1.2.1.25.1
view systemonly included .1.3
rocommunity public default -V systemonly
rocommunity6 public default -V systemonly
rouser authOnlyUser
sysLocation Sitting on the Dock of the Bay
sysContact Me <me@example.org>
sysServices 72
proc mountd
proc ntalkd 4
proc sendmail 10 1
disk / 10000
disk /var 5%
includeAllDisks 10%
load 12 10 5
trapsink localhost public
iquerySecName internalUser
rouser internalUser
defaultMonitors yes
linkUpDownNotifications yes
extend test1 /bin/echo Hello, world!
extend-sh test2 echo Hello, world! ; echo Hi there ; exit 35
master agentx
```
## แก้ไขเฉพาะ 3 ตัวหลัก
rocommunity ชื่อ community
syslocation ตั้งชื่อ
sysContact ชื่ออีเมล <email>;

เมื่อแก้ไขเสร็จสิ้นให้รันคำสั่ง
 
```
service snmpd stop
```
```
service snmpd start
```
```
service snmpd restart
```

## และรันคำสั่ง
 
```
snmpwalk -v2c -c ชื่อCommunity ipเครื่อง
```
***

## กำหนดชื่อ
## กำหนด ip
## กำหนด template
## กำหนด Host group
## กำหนด tool 
 
![image](https://user-images.githubusercontent.com/87377798/211206944-b499d552-6f43-4b35-b2ae-5569bbf10762.png)
 
 ## กำหนด macro
 
![image](https://user-images.githubusercontent.com/87377798/211207021-67f29eef-76ed-4e8b-b6ad-0f4d3f758e11.png)

***
 
เสร็จสิ้นการทำ SNMP

![Screenshot 2023-01-08 231127](https://user-images.githubusercontent.com/87377798/211207101-2101acdb-fae4-42a0-ab2f-68293abfd0f8.png)

***
