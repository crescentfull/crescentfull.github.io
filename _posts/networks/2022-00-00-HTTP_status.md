---
title: "HTTP 상태 코드"
description: "HTTP 상태 코드의 중요성과 각 상태 코드의 의미"
date: 2022-01-12 10:00:00 +0900
categories: [CS, Network]
tags: [Computer Science, Network]
# image: 
---

# HTTP 상태 코드란 무엇인가?

웹사이트에 접속할 때, 브라우저는 서버에게 정보를 요청한다. 이때 서버는 요청을 처리한 결과를 HTTP 상태 코드라는 숫자로 알려준다. 이 숫자는 요청이 성공했는지, 실패했는지, 실패했다면 왜 실패했는지를 알려주는 중요한 역할을 한다.

### HTTP 상태 코드의 중요성

HTTP 상태 코드는 `200 = 성공`, `400 = 클라이언트가 요청을 잘못 보냄`, `500 = 서버가 문제를 일으킴`과 같이 여러 가지 상황을 나타낸다. 웹사이트나 앱을 만들 때, 이 상태 코드를 잘 활용하면 프로그램이 더 안정적으로 작동할 수 있다.

### 브라우저와 상태 코드

브라우저는 서버가 보내주는 상태 코드를 보고 요청이 성공했는지 실패했는지를 판단한다. 예를 들어, 서버가 200번 상태 코드를 보내면 브라우저는 "아, 요청이 성공했구나!"라고 생각한다. 반면에 400번이나 500번 상태 코드를 받으면 "어, 뭔가 문제가 있네?"라고 판단한다.

### 각 상태 코드 알아보기

이제 각 상태 코드가 어떤 의미를 가지는지 하나씩 살펴본다. 모든 상태 코드를 다 알 필요는 없고, 자주 사용하는 것들만 알아두면 된다.

#### 100번대: 정보 제공

- **100 Continue**: 클라이언트가 요청을 계속해도 된다는 의미를 가진다.

  ````python
  @app.route('/continue')
  def continue_request():
      return '', 100
  ````

- **101 Switching Protocols**: 서버가 클라이언트의 프로토콜 전환 요청을 수락했음을 의미한다.

  ````python
  @app.route('/switch-protocol')
  def switch_protocol():
      return '', 101
  ````

#### 200번대: 성공

- **200 OK**: 요청이 성공적으로 처리되었음을 의미한다.

  ````python
  @app.route('/success')
  def success():
      return jsonify(message="요청이 성공했습니다!"), 200
  ````

- **201 Created**: 요청이 성공적으로 처리되었고, 새로운 리소스가 생성되었음을 의미한다.

  ````python
  @app.route('/create', methods=['POST'])
  def create():
      return jsonify(message="새로운 리소스가 생성되었습니다!"), 201
  ````

- **202 Accepted**: 요청이 수락되었지만, 아직 처리되지 않았음을 의미한다.

  ````python
  @app.route('/accepted')
  def accepted():
      return jsonify(message="요청이 수락되었습니다!"), 202
  ````

- **204 No Content**: 요청이 성공적으로 처리되었지만, 반환할 내용이 없음을 의미한다.

  ````python
  @app.route('/delete', methods=['DELETE'])
  def delete():
      return '', 204
  ````

#### 300번대: 리다이렉션

- **301 Moved Permanently**: 요청한 리소스가 다른 곳으로 옮겨졌음을 의미한다.

  ````python
  @app.route('/moved-permanently')
  def moved_permanently():
      return '', 301, {'Location': 'https://new-location.com'}
  ````

- **302 Found**: 요청한 리소스가 임시로 다른 곳에 있음을 의미한다.

  ````python
  @app.route('/found')
  def found():
      return '', 302, {'Location': 'https://temporary-location.com'}
  ````

- **304 Not Modified**: 요청한 리소스가 이전과 달라진 점이 없음을 의미한다.

  ````python
  @app.route('/not-modified')
  def not_modified():
      return '', 304
  ````

#### 400번대: 클라이언트 오류

- **400 Bad Request**: 클라이언트가 잘못된 요청을 보냈음을 의미한다.

  ````python
  @app.route('/bad-request')
  def bad_request():
      return jsonify(error="잘못된 요청입니다."), 400
  ````

- **401 Unauthorized**: 인증이 필요한 리소스를 인증 없이 요청했음을 의미한다.

  ````python
  @app.route('/unauthorized')
  def unauthorized():
      return jsonify(error="인증이 필요합니다."), 401
  ````

- **403 Forbidden**: 접근이 금지된 리소스를 요청했음을 의미한다.

  ````python
  @app.route('/forbidden')
  def forbidden():
      return jsonify(error="접근이 금지되었습니다."), 403
  ````

- **404 Not Found**: 요청한 리소스를 찾을 수 없음을 의미한다.

  ````python
  @app.route('/not-found')
  def not_found():
      return jsonify(error="리소스를 찾을 수 없습니다."), 404
  ````

- **405 Method Not Allowed**: 요청한 메소드가 허용되지 않음을 의미한다.

  ````python
  @app.route('/method-not-allowed', methods=['POST'])
  def method_not_allowed():
      return jsonify(error="허용되지 않는 메소드입니다."), 405
  ````

- **406 Not Acceptable**: 요청한 리소스가 클라이언트가 수용할 수 없는 형식임을 의미한다.

  ````python
  @app.route('/not-acceptable')
  def not_acceptable():
      return jsonify(error="수용할 수 없는 형식입니다."), 406
  ````

- **408 Request Timeout**: 서버가 클라이언트의 요청을 기다리다 시간이 초과되었음을 의미한다.

  ````python
  @app.route('/request-timeout')
  def request_timeout():
      return jsonify(error="요청 시간이 초과되었습니다."), 408
  ````

- **429 Too Many Requests**: 클라이언트가 너무 많은 요청을 보냈음을 의미한다.

  ````python
  @app.route('/too-many-requests')
  def too_many_requests():
      return jsonify(error="너무 많은 요청을 보냈습니다."), 429
  ````

#### 500번대: 서버 오류

- **500 Internal Server Error**: 서버에서 알 수 없는 오류가 발생했음을 의미한다.

  ````python
  @app.route('/server-error')
  def server_error():
      return jsonify(error="서버 오류가 발생했습니다."), 500
  ````

- **501 Not Implemented**: 서버가 요청한 기능을 지원하지 않음을 의미한다.

  ````python
  @app.route('/not-implemented')
  def not_implemented():
      return jsonify(error="기능이 구현되지 않았습니다."), 501
  ````

- **502 Bad Gateway**: 서버가 다른 서버로부터 잘못된 응답을 받았음을 의미한다.

  ````python
  @app.route('/bad-gateway')
  def bad_gateway():
      return jsonify(error="잘못된 게이트웨이입니다."), 502
  ````

- **503 Service Unavailable**: 서버가 요청을 처리할 준비가 되지 않았음을 의미한다.

  ````python
  @app.route('/service-unavailable')
  def service_unavailable():
      return jsonify(error="서비스를 사용할 수 없습니다."), 503
  ````

- **504 Gateway Timeout**: 서버가 다른 서버로부터 응답을 기다리다 시간이 초과되었음을 의미한다.

  ````python
  @app.route('/gateway-timeout')
  def gateway_timeout():
      return jsonify(error="게이트웨이 시간이 초과되었습니다."), 504
  ````

---

### 백엔드와 클라이언트 간의 문제 상황 예시

웹 애플리케이션을 개발할 때, 백엔드와 클라이언트 간에 다양한 문제 상황이 발생할 수 있다. 이러한 문제를 해결하기 위해 HTTP 상태 코드를 적절히 사용하는 것이 중요하다. 다음은 몇 가지 일반적인 문제 상황과 그에 대한 예시이다.

1. **잘못된 요청 데이터**: 클라이언트가 서버에 잘못된 데이터를 보낼 때, 서버는 `400 Bad Request` 상태 코드를 반환할 수 있다. 예를 들어, 필수 입력값이 누락된 경우이다.

   ````python
   @app.route('/submit', methods=['POST'])
   def submit():
       data = request.json
       if 'name' not in data:
           return jsonify(error="이름이 필요합니다."), 400
       return jsonify(message="데이터가 성공적으로 제출되었습니다."), 200
   ````

2. **인증 문제**: 사용자가 로그인하지 않고 보호된 리소스에 접근하려고 할 때, 서버는 `401 Unauthorized` 상태 코드를 반환할 수 있다.

   ````python
   @app.route('/profile')
   def profile():
       if not user_is_authenticated():
           return jsonify(error="로그인이 필요합니다."), 401
       return jsonify(profile_data)
   ````

3. **리소스 접근 제한**: 사용자가 권한이 없는 리소스에 접근하려고 할 때, 서버는 `403 Forbidden` 상태 코드를 반환할 수 있다.

   ````python
   @app.route('/admin')
   def admin():
       if not user_is_admin():
           return jsonify(error="관리자 권한이 필요합니다."), 403
       return jsonify(admin_data)
   ````

4. **리소스 없음**: 사용자가 존재하지 않는 페이지나 리소스를 요청할 때, 서버는 `404 Not Found` 상태 코드를 반환할 수 있다.

   ````python
   @app.route('/item/<int:item_id>')
   def get_item(item_id):
       item = find_item_by_id(item_id)
       if item is None:
           return jsonify(error="아이템을 찾을 수 없습니다."), 404
       return jsonify(item)
   ````

5. **서버 오류**: 서버에서 예기치 않은 오류가 발생할 때, `500 Internal Server Error` 상태 코드를 반환할 수 있다.

   ````python
   @app.route('/process')
   def process():
       try:
           # 처리 로직
           return jsonify(result="처리가 완료되었습니다."), 200
       except Exception as e:
           return jsonify(error="서버 오류가 발생했습니다."), 500
   ````

이러한 예시들은 백엔드와 클라이언트 간의 문제 상황을 처리하는 데 있어 HTTP 상태 코드가 어떻게 사용될 수 있는지를 보여준다. 적절한 상태 코드를 사용하면 클라이언트는 서버의 상태를 더 잘 이해하고, 사용자에게 적절한 피드백을 제공할 수 있다.

---

### 참고 자료
[Evans Library-about-http-status-code](https://evan-moon.github.io/2020/03/15/about-http-status-code/)