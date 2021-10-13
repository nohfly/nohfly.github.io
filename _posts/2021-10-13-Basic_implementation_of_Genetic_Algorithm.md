---
title: "# 1 Basic implementation of Genetic Algorithm"
categories:
 - project1
tags:
 - genetic algorithm
published: true
---

Genetic algorithm is one of search algorithms derived from Charles Darwin's theory of natural evolution. To survive in specific environments, individuals go through process of natural selection where the most fittest ones survive and produce offspring. During the reproduction, the genetic transcript of the parent is copied to the children but 

A genetic algorithm is **a search heuristic that is inspired by Charles Darwin's theory of natural evolution**. This algorithm reflects the process of natural selection where the fittest individuals are selected for reproduction in order to produce offspring of the next generation

## 서버에 python 서비스 등록
Django 서버 가동을 위한 .py 파일을 리눅스 서비스로 등록해, 서버 시작시 자동으로 구동되도록 함.

실행하고자 하는 python 파일
```python
# runserver.py
from waitress import serve  
from cogsvc.wsgi import application  
  
if __name__ == '__main__':  
    serve(application, port='8080')
```
### 1. .service 파일 작성
관리를 위해 해당 프로젝트 폴더 내 서비스이름.service 파일을 생성함
```
$ sudo vi /프로젝트경로/서비스이름.service
``` 

아래 내용 작성 후 :wq! 저장

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ0MzQ1OTg4NV19
-->