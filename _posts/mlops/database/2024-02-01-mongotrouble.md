---
title: MongoDB TroubleShooting
author: kiwing
date: 2024-02-01 20:12:28 +0800
categories: [MLOps, database]
tags: [DB]
pin: false
---

# Mongodb error 해결 과정

### DB를 Client에 Connected하는 과정에서 오류 발생해 mongdb 상태 확인
```
systemctl status mongod
```

<center>
  <img src="https://github.com/Ki-Wing/Ki-Wing.github.io/assets/92567807/37c2d4aa-7aa7-40bb-af57-5e54ee7ad838" alt="1" width="100%"/>
</center>
 
<br>

### disk space 체크
```
df -h 
```
- 192.168.xxx.xxx /dev/sda4  
- 오메...99% volume 확인
  - model checkpoint file 정리
  - training acc 30% 이하 정리 (대부분이 중간에 끊긴 파일이기 때문)

<br>

### mongod 실행해 system 및 mongo log 확인
```
sudo systemctl start mongod
```

- /var/log/mongodb/mongod.log

- /var/log/syslog

<br>

### log에서 dbpath error 발견   
```
// “error”:“NonExistentPath: Data directory /data/db not found. 
```

- Create the missing directory or specify another path using 
  
   (1) the --dbpath command line option
  
   (2) by adding the ‘storage.dbPath’ option in the configuration file

- dbpath 경로 변경  /data/db -> /data/datadb/lib/mongodb

```
mongod --dbpath=/data/datadb/lib/mongodb
```

<br>

### Failed to unlink socket file. Operation not permitted error 발생

- 원인 :  mongodb-27017.sock 파일에 설정된 접근권한이 MongoDB에게 허용 X
```
(1)
sudo rm -rf /tmp/mongodb-27017.sock
```
```
(2)
sudo chown -R mongodb:mongodb /var/lib/mongodb
sudo chown mongodb:mongodb /tmp/mongodb-27017.sock
```
<br>

## 최종
```
mongod --repair --dbpath /data/datadb/lib/mongodb 
```
<br>

오류 ) WiredTiger error message 
- WiredTiger.wt: potential hardware corruption, read checksum error for 20480B block at offset 65536
- /data/datadb/lib/mongodb/WiredTiger.turtle: handle-open: open: **Permission denied**

<br>

원인 ) root로 --repair 진행해, 소유권이 mongodb -> root로 변경됨

<br>

해결 )

```
cd /data/datadb/lib/mongodb/
sudo chown -R mongodb:mongodb Wired*
sudo chown -R mongodb:mongodb journal/
```

<center>
  <img src="https://github.com/Ki-Wing/Ki-Wing.github.io/assets/92567807/2849c78c-4c32-41e1-bcea-14371d85745b" alt='2' width="100%"/>
</center>
 
<br>

<br>

+) data (pile, flan) <br>
{"t":{"$date":"2024-02-01T15:32:33.795+09:00"},"s":"I",  "c":"INDEX",    "id":20303,   "ctx":"initandlisten","msg":"validating collection","attr":{**"namespace":"corpus.pile"**,"uuid":{"uuid":{"$uuid":"ad934065-4e43-4b67-8403-e8b6bc90aa29"}}}} <br>
{"t":{"$date":"2024-02-01T15:32:39.149+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":100,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:32:44.144+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":200,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:32:49.083+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":300,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:32:54.046+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":400,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:32:59.184+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":500,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:33:04.667+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":600,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:33:10.192+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":700,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:33:15.022+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":800,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:33:19.976+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":900,"total":140505,"percent":0}}
{"t":{"$date":"2024-02-01T15:33:26.232+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":1000,"total":140505,"percent":0}}
…

{"t":{"$date":"2024-02-01T13:27:31.318+09:00"},"s":"I",  "c":"INDEX",    "id":20303,   "ctx":"initandlisten","msg":"validating collection","attr":{**"namespace":"corpus.flan"**,"uuid":{"uuid":{"$uuid":"3fae256c-1c98-40c6-9a3b-e3c8abeea6dc"}}}}<br>
{"t":{"$date":"2024-02-01T13:27:34.779+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":200,"total":411,"percent":48}}
{"t":{"$date":"2024-02-01T13:27:37.182+09:00"},"s":"I",  "c":"-",        "id":51773,   "ctx":"initandlisten","msg":"progress meter","attr":{"name":"Validate: scanning documents","done":300,"total":411,"percent":72}}
…

[nodejs]: https://nodejs.org/
[starter]: https://github.com/cotes2020/chirpy-starter
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow
[latest-tag]: https://github.com/cotes2020/jekyll-theme-chirpy/tags
