---
title: "Django project 초기세팅"
description: "Django project 초기 세팅을 하는 방법"
date: 2022-01-04 10:00:00 +0900
categories: [Django]
tags: [Django]
---


# Django Project를 위한 초기세팅

### 가상환경 생성
프로젝트 마다 독립적인 패키지의 관리를 위해 새로운 가상환경을 생성해서 사용!
```bash
#가상환경 생성
conda create -n "가상환경 이름" python=3.8
conda activate "가상환경 이름"
```
### Database 생성
**mysql을 미리 설치해야 한다**
```sql
$ mysql -u root -p
명령어 입력 후 mysql로 전환

mysql> create database NAME character set utf8mb4 collate utf*mb4_general_ci; 
```

### Project Python Package 설치
```bash
$ pip install django

# 이후에 MySQL server에 접속하기 위한 package
$ pip install mysqlclient
  (중요) mysql 설치되어 있는지 먼저 확인

# 맥북은 mysqlclient를 다운받으려면 오류가 난다 그러므로 PyMySQL을 다운받자
$ pip install PyMySQL

#!밑글에 pymysql 세팅법을 적어놓음
```

### Django Project 생성

```bash
$ django-admin startproject westarbucks
$ cd westarbucks

```

### Settings.py 설정
. IP허용
```python
	ALLOWED_HOSTS = ['*']
```
. 추가로 westarbucks/urls.py를 아래와 같이 수정
```	python
from django.urls import path
urlpatterns = [
]

```
. 주석처리(admin, csrf, auth)
![](https://images.velog.io/images/sicksong/post/dcbcd035-dd5c-419e-942a-b74cf1a43185/image.png)

### my_settings.py 생성(DATABASES,SECRET_KEY)
. django 설정에 존재하는 내용 중 **SECRET_KEY, DATABASE 등**은 절대 settings.py에 두면 안됩니다.. 그대로 깃이나 오픈클라우드에 올려버리면 **해킹을 당하기가 쉽습니다. 아니 당합니다!**
. 그래서 별도의 참조용 파이썬 파일(my_settings.py)을 생성해서, 참조하는 방법으로 진행
```bash
cd '생성한 프로젝트 폴더명'
touch my_settings.py
```
. 파일에 실제 쓰여지는 내용
```python
#예시
DATABASES = {
    'default' : {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'DATABASE 명',
        'USER': 'DB접속 계정명',
        'PASSWORD': 'DB접속용 비밀번호',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}

SECRET_KEY = '시크릿키' #settings.py에 있는 secret_key 를 사용합니다.
```
### settings.py <-> my_settings.py 연동
```python
#intel 맥북

from pathlib		import Path #기존에 settings.py에 있는 코드
from my_settings	import DATABASES, SECRET_KEY

DATABASES = DATABASES

SECRET_KEY = SECRET_KEY

#### m1 맥북!!
# $ pip install PyMySQL
# 으로 PyMySQL을 다운받는거 잊지말자

from pathlib        import Path #기존에 settings.py 에 있는 코드
from my_settings import DATABASES, SECRET_KEY

import pymysql

pymysql.install_as_MySQLdb()

```

.corsheaders
```bash
pip install django-cors-headers
```
. 설치했다면 settings.py에 INSTALLED_APPS 안에 추가

```python
INSTALLED_APPS = [
...
		'corsheaders'
]
```
. middleware 추가
```python
MIDDLEWARE = [
	...
		'corsheaders.middleware.CorsMiddleware',
	...
]
```
. CORS 추가 설정
```python
##CORS
CORS_ORIGIN_ALLOW_ALL=True
CORS_ALLOW_CREDENTIALS = True

CORS_ALLOW_METHODS = (
    'DELETE',
    'GET',
    'OPTIONS',
    'PATCH',
    'POST',
    'PUT',
)

CORS_ALLOW_HEADERS = (
    'accept',
    'accept-encoding',
    'authorization',
    'content-type',
    'dnt',
    'origin',
    'user-agent',
    'x-csrftoken',
    'x-requested-with',
)
```
### 서버 정상 동작 확인
. 서버 동작(Runserver)을 통한 오류 검증
```
python manage.py runserver
```
. 정상 동작 예시
![](https://images.velog.io/images/sicksong/post/c5b49832-8cbf-4b05-a623-b8400ce55e77/image.png)


이제 프로젝트를 위한 초기 세팅은 끝났다~
프로젝트를 진행해보자!