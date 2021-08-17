---
title: "Django model에 Oracle Database 연동"
categories:
 - Django
tags:
 - Django
published: true
---

## Oracle DB Connection 정보 설정
+ **settings.py 설정**
```python
DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.oracle',
		'NAME': '***',  # SID
		'USER': '***',
		'PASSWORD': '***',
		'HOST': 'asdf.qwer.ap-northeast-2.rds.amazonaws.com',  # Endpoint 또는 host명
		'PORT': '1521'
	}
}
```
Django 프로젝트 내 settings.py 안에서 'DATABASES' 항목을 작성한다. AWS RDS DB 인스턴스에 접근할 경우, host ip가 변경될 수도 있기 때문에 Endpoint 경로를 입력한다.

+ **cx_Oracle 설치**
django.db.backends.oracle은 cx_Oracle을 사용하기에 별도로 라이브러리를 설치해준다.
```
conda install cx_oracle
```

+ **connection 여부 확인**
설정한 DATABASE에 잘 접근하는지 확인한다.
```
python manage.py shell
>>> from django.db import connection
>>> cursor = connection.cursor()
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM2MTczNTk5OSwtMTIzNzQxMDUzNCwyMD
k5MzA3MDY5LC0yMDQ0MDE2OTA5LC0xODM3ODg2NDc3LC0xNDIz
MjY2MDY1XX0=
-->