---
title: "Django model에 Oracle Database 연동"
categories:
 - Django
tags:
 - Django
published: true
---

## Django - Oracle DB 연동
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
```
conda install cx_Oracle
```
django.db.backends.oracle은 cx_Oracle을 사용하기에 별도로 라이브러리를 설치해준다.

+ **connection 여부 확인**
```
python manage.py shell
>>> from django.db import connection
>>> cursor = connection.cursor()
```
설정한 DATABASE에 잘 접근하는지 확인한다.
> django.db.utils.DatabaseError: DPI-1047: Cannot locate a 64-bit Oracle Client library: "dlopen(libclntsh.dylib, 1): image not found". See https://cx-oracle.readthedocs.io/en/latest/user_guide/installation.html for help

애러가 난다...

Oracle DB와의 32-bit/64-bit 불일치 문제가 아니라면 Oracle Instant Client를 설치하지 않았거나, 설치는 했으나 cx_Oracle에서 해당 경로를 인식하지 못해서 발생하는 문제
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2OTQ3NzczMiwtMTIzNzQxMDUzNCwyMD
k5MzA3MDY5LC0yMDQ0MDE2OTA5LC0xODM3ODg2NDc3LC0xNDIz
MjY2MDY1XX0=
-->