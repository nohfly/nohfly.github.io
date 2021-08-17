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

---
+ **cx_Oracle 설치**
```
conda install cx_Oracle
```
django.db.backends.oracle은 cx_Oracle을 사용하기에 별도로 라이브러리를 설치해준다.

---
+ **connection 여부 확인**
```python
python manage.py shell
>>> from django.db import connection
>>> cursor = connection.cursor()
```
설정한 DATABASE에 잘 접근하는지 확인한다.

---
+ **Oracle Instant Client 설정**
> django.db.utils.DatabaseError: DPI-1047: Cannot locate a 64-bit Oracle Client library: "dlopen(libclntsh.dylib, 1): image not found". See https://cx-oracle.readthedocs.io/en/latest/user_guide/installation.html for help

애러가 난다...

Oracle DB와의 32-bit/64-bit 불일치 문제가 아니라면 Oracle Instant Client를 설치하지 않았거나, 설치는 했으나 cx_Oracle에서 해당 경로를 인식하지 못해서 발생하는 문제이다.

[`Oracle Instant Client 다운로드`](https://www.oracle.com/database/technologies/instant-client/downloads.html)

위의 경로에서 OS에 맞게 해당 라이브러리를 다운받고, [*cx_Oracle 8 Installation*](https://cx-oracle.readthedocs.io/en/latest/user_guide/installation.html#) 설명에 따라 라이브러리를 설정한다.

```python
import cx_Oracle
cx_Oracle.init_oracle_client(lib_dir="/Users/your_username/Downloads/instantclient_19_8")
```
macOS의 경우, 위와 같이 Oracle Instant Client 다운로드 후 해당 라이브러리 경로를 파이선 실행 코드 내에서 한번 설정해줘야한다고 하는데,
```python
# .../lib/python3.7/site-packages/django/db/backends/oracle/base.py

try:  
    import cx_Oracle as Database  
except ImportError as e:  
    raise ImproperlyConfigured("Error loading cx_Oracle module: %s" % e)
```
Django 내에서 cx_Oracle을 불러오는 위치가 저 안에 있어서 해당 코드를 직접 수정하지 않는 이상 불가능해 보인다. 때문에 차선책으로 직접 Oracle Instant Client 라이브러리를 cx_Oracle이 인식할 수 있는 `/usr/local/lib`으로 옯겨준다.

---
+ **Oracle Instant Client 설정 (macOS)**
	+ [Oracle](https://www.oracle.com/database/technologies/instant-client/macos-intel-x86-downloads.html)에서 Basic 64-bit DMG를 다운 받은 후 마운트 한다.
	+ 터미널에서 아래 설치 스크립트를 실행한다.
/Volumes/instantclient-basic-macos.x64-19.8.0.0.0dbru/install_ic.sh
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMzMjgwNzE1MywyMDI5OTkwODQsLTEyMz
c0MTA1MzQsMjA5OTMwNzA2OSwtMjA0NDAxNjkwOSwtMTgzNzg4
NjQ3NywtMTQyMzI2NjA2NV19
-->