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

```


```
$ ssh username@원격서버주소 -p 포트번호
```
-- 22번 이외의 포트번호 사용시..

---
### 2. SSH Tunneling 설정하기
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ1NzM0NjM1N119
-->