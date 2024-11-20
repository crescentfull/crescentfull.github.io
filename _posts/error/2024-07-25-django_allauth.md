---
title: "django-allauth"
description: "django-allauth"
date: 2024-07-25 10:00:00 +0900
categories: [Error]
tags: [Error, django, allauth]
---

# django-allauth
```bash
ModuleNotFoundError: No module named 'allauth.account.context_processors'
[25/Jul/2024 20:19:40] "GET /admin/login/?next=/admin/ HTTP/1.1" 500 137666
```

django-allauth를 사용할 때 
settings.py의 templates에서 "allauth.account.context_processors.account" 를 적으면 안된다.

왜냐하면 allauth의 context_processors는 2015년부터 사용하지 않는 패키지다..

gpt는 이 사실을 모르고 있더라..!

패키지 설치 폴더까지가서 찾아봤지만 context_processors파일이 존재하지 않길래 구글링해본 결과 stackoverflow에서 더이상 쓰지 않으니 지우라는 글을 찾았다.

https://stackoverflow.com/questions/38018931/django-allauth-importerror-at-admin-no-module-named-context-processors

하하ㅏ하하하하하ㅏㅏ

```python
/setting.py

TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.debug",
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
                #"allauth.account.context_processors.account",
                #'allauth.socialaccount.context_processors.socialaccount'
            ],
        },
    },
]
```

다시 저장한뒤에 runserver로 admin페이지에 접속하니

```bash
[25/Jul/2024 21:27:55] "GET / HTTP/1.1" 200 3
[25/Jul/2024 21:28:01] "GET /admin/ HTTP/1.1" 302 0
[25/Jul/2024 21:28:01] "GET /admin/login/?next=/admin/ HTTP/1.1" 200 4194
[25/Jul/2024 21:28:01] "GET /static/admin/css/dark_mode.css HTTP/1.1" 200 2682
[25/Jul/2024 21:28:01] "GET /static/admin/css/nav_sidebar.css HTTP/1.1" 200 2810
[25/Jul/2024 21:28:01] "GET /static/admin/css/base.css HTTP/1.1" 200 21544
[25/Jul/2024 21:28:01] "GET /static/admin/css/login.css HTTP/1.1" 200 958
[25/Jul/2024 21:28:01] "GET /static/admin/js/theme.js HTTP/1.1" 200 1943
[25/Jul/2024 21:28:01] "GET /static/admin/css/responsive.css HTTP/1.1" 200 17905
[25/Jul/2024 21:28:01] "GET /static/admin/js/nav_sidebar.js HTTP/1.1" 200 3063
[25/Jul/2024 21:28:13] "POST /admin/login/?next=/admin/ HTTP/1.1" 302 0
[25/Jul/2024 21:28:13] "GET /admin/ HTTP/1.1" 200 9589
[25/Jul/2024 21:28:13] "GET /static/admin/css/dashboard.css HTTP/1.1" 200 441
[25/Jul/2024 21:28:13] "GET /static/admin/img/icon-changelink.svg HTTP/1.1" 200 380
[25/Jul/2024 21:28:13] "GET /static/admin/img/icon-addlink.svg HTTP/1.1" 200 331
```