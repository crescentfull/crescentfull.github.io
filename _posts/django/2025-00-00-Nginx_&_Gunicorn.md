---
title: "macOS에서 Nginx·Gunicorn으로 Django 배포하기 #1"
description: "Nginx·Gunicorn 개념 설명"
date: 2025-05-01 10:00:00 +0900
categories: [Django, Deployment]
tags: [Django, Nginx, Gunicorn, macOS]
---

이번에 새롭게 토이프로젝트를 진행하면서 실제 사용자들을 위해서 서버 배포를 해야할 일이 생겼다.
비용 문제로 pythonanywhere나 render 같은 호스팅 사이트를 알아보는 중에,
일단 배포를 하기 위해서 먼저해야할 것들을 찾다가 nginx 와 gunicorn에 대해 제대로 짚고 넘어가야할 것 같아서 블로그 포스팅을 한다.

*Django 프로젝트이기 때문에 Django중심으로 

## 1. Nginx & Gunicorn이란?  

파이썬으로 만들어진 프로젝트를 배포할 Nginx + Gunicorn의 조합을 쓰는 것이 표준이자 권장되고 있다.  
안정적인 HTTP 처리와 프로세스관리를 위해서 Gunicorn을 사용하고 거기에 정적파일과 미디어 파일등을 효율적으로 처리하기 위해 Nginx를 사용한다. 

### 1) Nginx

Nginx는 **높은 동시 접속 처리 능력**과 **리버스 프록시(reverse proxy)** 기능으로 유명한 웹 서버이다. 
정적 파일(이미지, CSS, JS)을 빠르게 전달하고, 동적 요청은 뒤쪽 애플리케이션 서버(여기서는 Gunicorn)로 프록시해 주는 역할을 한다.

- **주요 특징**
  1. 이벤트-기반(Event-driven) 아키텍처 → 높은 처리량(throughput)
  2. 로드 밸런싱, 캐싱, SSL 종료(termination) 등 다양한 기능 내장
  3. 설정 파일이 직관적이고, 재시작 없이 설정을 재적용(`nginx -s reload`)할 수 있음

> 더 자세한 내용은 Nginx 공식 문서를 참고하세요.  
> 👉 [Nginx 공식 문서 바로가기](https://nginx.org/en/docs/)

---

### 2) Gunicorn

Gunicorn(Green Unicorn)은 **WSGI(웹 서버 게이트웨이 인터페이스)** 표준을 구현한 Python 애플리케이션 서버이다.
Django, Flask 같은 WSGI 호환 프레임워크와 함께 사용하여 **Python 코드(동적 요청)** 를 실행한다.

- **주요 특징**
  1. 프리포크(pre-fork) 모델: 마스터 프로세스가 여러 워커(worker)를 미리 띄워 요청을 병렬 처리  
  2. 설정이 간단하며, CLI 옵션으로 워커 수·타임아웃 등을 손쉽게 조정  
  3. Uvicorn(ASGI)과 달리 **WSGI 전용**이므로, 전통적인 Django 앱에 적합

> 더 자세한 내용은 Gunicorn 공식 문서를 참고하세요.  
> 👉[Gunicorn 공식 문서 바로가기](https://gunicorn.org/#docs)
---

### 3) Nginx + Gunicorn 조합

1. 사용자가 브라우저에서 요청  
2. **Nginx**  
   - 정적 파일이면 직접 응답  
   - 동적 요청이면 **Gunicorn**으로 프록시  
3. **Gunicorn**이 Django 코드를 실행해 응답 생성  
4. 응답이 Nginx를 거쳐 사용자에게 전달  

이 구조를 통해  
- 정적 자원은 Nginx가 고성능으로 처리  
- 동적 로직은 Gunicorn(+Django)이 담당하여 **효율적인 리소스 분배**와 **보안적인 분리**를 달성할 수 있다.

