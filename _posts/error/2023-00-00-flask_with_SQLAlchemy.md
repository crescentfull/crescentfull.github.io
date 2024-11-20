---
title: "Flask with SQLAlchemy"
description: "Flask with SQLAlchemy"
date: 2023-12-20 10:00:00 +0900
categories: [Error]
tags: [Error, Flask, SQLAlchemy]
image: https://velog.velcdn.com/images/sicksong/post/fb267989-5695-407f-97b5-f607f89cabc2/image.png
---

<!-- ![](https://velog.velcdn.com/images/sicksong/post/fb267989-5695-407f-97b5-f607f89cabc2/image.png) -->

## "Working outside of application context"

 Flask 애플리케이션 컨텍스트 외부에서 작업을 시도했다는 것을 의미
 Flask에서는 특정 작업들이 애플리케이션 컨텍스트 내에서 실행되어야 한다.
 이 컨텍스트는 Flask가 실행 중인 동안 애플리케이션과 관련된 정보에 접근할 수 있게 해준다.
이 문제를 해결하기 위해, create_all() 메서드를 호출하기 전에 Flask 애플리케이션 컨텍스트를 생성해야 한다. 

>with app.app_context(): 구문을 사용
 
## code
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

# Flask 애플리케이션 인스턴스 생성
app = Flask(__name__)

##CREATE DATABASE
# 'SQLALCHEMY_DATABASE_URI'는 사용하는 데이터베이스에 맞게 설정
# 예: SQLite를 사용하는 경우 'sqlite:///example.db'
app.config['SQLALCHEMY_DATABASE_URI'] = "sqlite:///new-books-collection.db"
#Optional: But it will silence the deprecation warning in the console.
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# SQLAlchemy 인스턴스 생성
db = SQLAlchemy(app)


##CREATE TABLE
class Book(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(250), unique=True, nullable=False)
    author = db.Column(db.String(250), nullable=False)
    rating = db.Column(db.Float, nullable=False)

    
    # __repr__(self): 메서드는 Python에서 클래스의 인스턴스를 문자열로 표현하는 방법을 정의. 
    # 이 메서드는 클래스의 객체가 어떻게 출력될지를 결정하며, 주로 디버깅과 로깅 목적으로 사용
    def __repr__(self):
        return f'<Book {self.title}>'

#✅ 애플리케이션 컨텍스트 내에서 데이터베이스 테이블 생성
# 이렇게 함으로써 Flask 애플리케이션 설정에 접근할 수 있음    
with app.app_context():
    db.create_all()



```