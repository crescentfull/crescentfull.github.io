---
title: "WebSocket 그리고 HTTP"
description: "WebSockek과 HTTP로 알아보는 웹 통신"
date: 2024-12-24 10:00:00 +0900
categories: [CS, Network]
tags: [Computer Science, Network, WebSocket, HTTP]
image: https://github.com/user-attachments/assets/259e3a20-6e14-40c7-848c-924009b1d6f8
---

## WebSocket과 HTTP

### WebSocket
**WebSocket**은 웹 브라우저와 서버 간의 [양방향 통신](https://en.wikipedia.org/wiki/Duplex_(telecommunications))을 가능하게 하는 프로토콜이다.  
-   기존의 HTTP 프로토콜은 요청-응답 방식으로 동작하지만, WebSocket은 **단일 TCP 연결을 통해 양방향 통신**을 지원한다.
그렇기 때문에 비교적 최신 버전의 인앱 채팅, 알림, 음성 또는 화상통화와 같은 실시간 애플리케이션에 적합하다.

HTTP와 동일하게 **애플리케이션 계층**에서 동작한다. 그리고 [평문](https://developer.mozilla.org/ko/docs/Glossary/Plaintext) 메시지 전송 방식이므로, [SSL/TLS](https://aws.amazon.com/ko/compare/the-difference-between-ssl-and-tls/) 보안 계층으로 암호화 되어야 데이터 탈취를 방지할 수 있다.

#### **WebSocket의 특징**
- **양방향 통신**: 클라이언트와 서버가 서로 데이터를 주고받을 수 있다.
- **실시간 데이터 전송**: 지연 시간이 적고, 실시간으로 데이터를 주고받을 수 있다.
- **단일 연결**: 초기 핸드셰이크 이후 단일 TCP 연결을 통해 통신이 이루어진다.
- **경량 프로토콜**: HTTP보다 오버헤드가 적고, 효율적인 데이터 전송이 가능하다.

### HTTP(HyperText Transfer Protocol)
- 문자 그대로 하이퍼텍스트 전송 프로토콜인 HTTP는 하이퍼텍스트 링크를 사용하여 웹페이지를 로드하는데 사용된다.
- 클라이언트와 서버 간의 비연속적(stateless) 요청-응답 방식의 통신 프로토콜이다.
- 클라이언트가 서버에 요청(request) -> 서버가 응답(response) 반환
- 단방향 통신이므로 서버가 클라이언트에 데이터를 보낼 수 없다.
그렇기에 주로 웹페이지 로드, API 호출 등에 사용된다.

### WebSocket의 동작 원리
![WebSocket&HTTP](https://github.com/user-attachments/assets/259e3a20-6e14-40c7-848c-924009b1d6f8)

WebSocket은 **HTTP 핸드셰이크**를 통해 연결을 설정한 후, **단일 TCP 연결**을 통해 통신을 유지한다.
즉, 초기 핸드셰이크 이후 단일 TCP 연결을 통해 통신이 이루어진다.

#### **핸드셰이크 과정**
1. 클라이언트가 서버에 HTTP 요청을 보내면서 WebSocket 연결 요청한다.
    -   HTTP의 upgrade헤더를 사용하여 WebSocket 연결을 요청
2. 서버가 요청을 승인하면, WebSocket 연결이 설정된다.
    -   서버는 101 Switching Protocols 응답을 보내면서 프로토콜을 변경
3. 이후 클라이언트와 서버는 양방향으로 데이터를 주고받을 수 있다.
    - HTTP 요청-응답 방식이 아닌 지속적인 연결 상태에서 데이터를 주고 받는 것

### WebSocket Handshake 테스트
...


---

## 참고
- [WebSocket](https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API)
- [HTTP](https://developer.mozilla.org/ko/docs/Web/HTTP)
- [WebSocket vs HTTP](https://velog.io/@sangje112/http-vs-WebSocket-http-%ED%86%B5%EC%8B%A0%EA%B3%BC-WebSocket%EC%9D%98-%EC%9E%A5%EB%8B%A8%EC%A0%90)
- [WebSocket 동작원리](https://kellis.tistory.com/65)