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
*Oracle DB endpoint 정보 'HOST'에 입력*
```python
DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.oracle',
		'NAME': '***',
		'USER': '***',
		'PASSWORD': '***',
		'HOST': 'asdf.qwer.ap-northeast-2.rds.amazonaws.com',
		'PORT': '1521'
	}
}
```

+ **cx_oracle 설치**
```
conda install cx_oracle
```

+ **connection 확인**
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAzMTIyNzAyMiwyMDk5MzA3MDY5LC0yMD
Q0MDE2OTA5LC0xODM3ODg2NDc3LC0xNDIzMjY2MDY1XX0=
-->