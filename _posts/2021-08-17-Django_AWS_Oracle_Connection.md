---
title: "Django model에 Oracle Database 연동"
categories:
 - Django
tags:
 - Django
published: true
---

## settings.py 설정
```python
DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.oracle',  
		'NAME': 'ORCL',  
  'USER': 'admin',  
  'PASSWORD': 'qkffprtm123',  
  'HOST': 'vs-dev-seoul-oracle-se2.c3vdxjip5vfb.ap-northeast-2.rds.amazonaws.com',  
  'PORT': '1521'  
  }  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg0NDE1NTk2OSwyMDk5MzA3MDY5LC0yMD
Q0MDE2OTA5LC0xODM3ODg2NDc3LC0xNDIzMjY2MDY1XX0=
-->