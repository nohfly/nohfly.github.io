---
title: "systemctl 메모"
categories:
 - systemctl
tags:
 - systemctl
published: true
---

## 서버에 python 서비스 등록
Django 서버 가동을 위한 .py 파일을 리눅스 서비스로 등록해, 서버 시작시 자동으로 구동되도록 함.

실행하고자 하는 python 파일
```python
# runserver.py
from waitress import serve  
from cogsvc.wsgi import application  
  
if __name__ == '__main__':  
    serve(application, port='8080')
```
### 1. .service 파일 작성
관리를 위해 해당 프로젝트 폴더 내 서비스이름.service 파일을 생성함
```
$ sudo vi /프로젝트경로/서비스이름.service
``` 

아래 내용 작성 후 :wq! 저장
```
[Unit]  
Description = Fitness Measure  
After = multi-user.target  
  
[Service]  
Type = simple  
WorkingDirectory = /home/usera/FitnessMeasure  
ExecStart = /home/usera/anaconda3/envs/AAAService/bin/python /home/usera/FitnessMeasure/runserver.py  
Restart = on-failure  
RestartSec = 10s  
  
[Install]  
WantedBy = multi-user.target
```
### 2. .service 파일의 권한 수정
```
$ sudo chmod 755 /프로젝트경로/서비스이름.service
```
### 3. systemd 디렉토리에 링크 생성
```
$ sudo ln -s /프로젝트경로/서비스이름.service /etc/systemd/system/서비스이름.service
```
### 4. 서비스 자동 실행 등록
```
$ sudo systemctl daemon-reload
$ sudo systemctl enable 서비스이름.service
```
### 5. 서비스 수동 실행 및 상태 확인
```
$ sudo systemctl start 서비스이름.service
$ sudo systemctl status 서비스이름.service
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY5MDU4NTcxN119
-->