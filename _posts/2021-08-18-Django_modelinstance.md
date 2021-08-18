---
title: "Django model instance  내용 정리"
categories:
 - Django
tags:
 - Django
 - Django model
 - cx_Oracle
 - Oracle Instant Client
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

---

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU0OTc1OTAxMV19
-->