---
title: "Django model에 Oracle Database 연동"
categories:
 - Django
tags:
 - Django
 - Oracle
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
+ **Oracle Instant Client 설정 (linux)**
	+ [Oracle](https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html)에서 Basic 64-bit rpm을 다운 받는다.
	+ 터미널에서 아래 설치 스크립트를 실행한다.
		```
		sudo yum install oracle-instantclient-basic-21.1.0.0.0-1.x86_64.rpm
		```
	+ 기본적으로 `$HOME/Downloads/instantclient_19_8`에 라이브러리가 복사되며, `/usr/local/oracle`로 이동시킨다.
		```
		sudo mkdir /usr/local/oracle
		sudo mv ~/Downloads/instantclient_19_8 /usr/local/oracle
		```

---
+ **Oracle Instant Client 설정 (macOS)**
	+ [Oracle](https://www.oracle.com/database/technologies/instant-client/macos-intel-x86-downloads.html)에서 Basic 64-bit DMG를 다운 받은 후 마운트 한다.
	+ 터미널에서 아래 설치 스크립트를 실행한다.
		```
		/Volumes/instantclient-basic-macos.x64-19.8.0.0.0dbru/install_ic.sh
		```
	+ 자동으로 path 경로와 필요 라이브러리가 같이 설치 되지만, rpm으로 설치하지 않거나, Instant Client 19 이전 버전을 설치 할 경우, 하단의 링크를 참조한다.일 경우,기본적으로 `$HOME/Downloads/instantclient_19_8`에 라이브러리가 복사되며, `/usr/local/oracle`로 이동시킨다.
		```
		sudo mkdir /usr/local/oracle
		sudo mv ~/Downloads/instantclient_19_8 /usr/local/oracle
		```
	+ `/usr/local/lib`으로 심볼릭 링크를 설정한다.
		```
		ln -sf /usr/local/oracle/instantclient_19_8/*.dylib /usr/local/lib/
		ln -sf /usr/local/oracle/instantclient_19_8/*.dylib.19.1 /usr/local/lib/
		ln -sf /usr/local/oracle/instantclient_19_8/libclntsh.dylib.19.1 /usr/local/lib/libclntsh.dylib
		```
	+ 다시 `cursor = connection.cursor()`로 connection 여부를 확인하면 애러 메시지가 출력되지 않는 것을 확인할 수 있다.

		[참조] [Python에서 Oracle 사용하기 (for Mac OS X)](https://davelogs.tistory.com/23)  
		[참조] [Python에서 Oracle 사용하기 (for Linux)](https://davelogs.tistory.com/24)   

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU1MTgyOTk2NywxMjY0MjYwMTMzLDE5Nj
I5Njk2OTYsMjI0MzExNTM0LC03MTkyMjgxNTgsNzEzOTc1NzAs
NTg1Njc0NjM4LDIwMjk5OTA4NCwtMTIzNzQxMDUzNCwyMDk5Mz
A3MDY5LC0yMDQ0MDE2OTA5LC0xODM3ODg2NDc3LC0xNDIzMjY2
MDY1XX0=
-->