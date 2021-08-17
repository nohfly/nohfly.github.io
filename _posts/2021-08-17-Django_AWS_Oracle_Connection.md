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
Django 프로젝트 내 settings.py 안에서 'DATABASES' 항목을 작성한다. ㄸ
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

+ **cx_oracle 설치**
```
conda install cx_oracle
```

+ **connection 여부 확인**
```
python manage.py shell
>>> from django.db import connection
>>> cursor = connection.cursor()
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMDgxNDEzNzYsLTEyMzc0MTA1MzQsMj
A5OTMwNzA2OSwtMjA0NDAxNjkwOSwtMTgzNzg4NjQ3NywtMTQy
MzI2NjA2NV19
-->