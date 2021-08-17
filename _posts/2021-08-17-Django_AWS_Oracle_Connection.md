---
title: "Django model에 Oracle Database 연동"
categories:
 - Django
tags:
 - Django
published: true
---

## settings.py 설정
'''
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
'''
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA5OTMwNzA2OSwtMjA0NDAxNjkwOSwtMT
gzNzg4NjQ3NywtMTQyMzI2NjA2NV19
-->