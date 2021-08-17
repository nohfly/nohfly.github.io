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
		'NAME': '***',
		'USER': '***',
		'PASSWORD': '***',
		'HOST': 'asdf.qwer.ap-northeast-2.rds.amazonaws.com',
		'PORT': '1521'
	}
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNzEzMTQ5NDYsMjA5OTMwNzA2OSwtMj
A0NDAxNjkwOSwtMTgzNzg4NjQ3NywtMTQyMzI2NjA2NV19
-->