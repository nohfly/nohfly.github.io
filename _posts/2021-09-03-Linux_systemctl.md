---
title: "systemctl 메모"
categories:
 - systemctl
tags:
 - systemctl
published: true
---

## 서버에 python 서비스 등록
Django 서버 가동을 위한 .py 파일을 리눅스 서비스로 등록해, 서버 시작시 자동으로 구동되도록 함.

실행하고자 하는 python 파일
```python
$ ssh username@원격서버주소
```
-- 22번 포트로 접속한다.

```
$ ssh username@원격서버주소 -p 포트번호
```
-- 22번 이외의 포트번호 사용시..

---
### 2. SSH Tunneling 설정하기
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk4MjY5NTI2N119
-->