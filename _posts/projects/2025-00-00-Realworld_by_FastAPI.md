---
title: "Realworld with FastAPI"
description: "Realworld by FastAPI + layerd Architecture, TDD 도입시도"
date: 2025-11-29 10:00:00 +0900
categories: [Projects, Project]
tags: [FastAPI, realworld, cursor, git, postgreSQL]
image: https://docs.realworld.show/_astro/realworld-logo.aLmMVRkK_2jCAtJ.webp
---

# 현재 시스템 아키텍처 및 흐름도

## 1. 전체 레이어 구조

```
┌─────────────────────────────────────────────────────────┐
│                    Client (HTTP)                         │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   API Layer (Router)                     │
│  - app/api/auth.py                                       │
│  - app/api/profile.py                                    │
│  책임: HTTP 요청/응답 처리, 라우팅                          │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│              Core (의존성, 보안, DB 설정)                  │
│  - app/core/dependencies.py (get_current_user)          │
│  - app/core/security.py (JWT 생성/검증)                  │
│  - app/core/database.py (DB 연결)                        │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                  Service Layer                           │
│  - app/services/user_service.py                          │
│  - app/services/profile_service.py                       │
│  책임: 비즈니스 로직, 검증, 여러 Repository 조합           │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                Repository Layer                          │
│  - app/repositories/user_repository.py                   │
│  - app/repositories/follow_repository.py                 │
│  책임: 데이터베이스 접근 (CRUD)                            │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   Model Layer                            │
│  - app/models/user_model.py                              │
│  - app/models/follow_model.py                            │
│  책임: 데이터 구조 정의 (SQLModel)                         │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                Database (SQLite)                         │
└─────────────────────────────────────────────────────────┘
```

---

## 2. 인증 흐름 (회원가입/로그인)

### 2.1 회원가입 (POST /users)

```
Client
  │
  │ POST /users
  │ { "user": { "email": "...", "password": "...", "username": "..." } }
  ▼
auth.py: register()
  │
  │ 1. UserRegisterRequest 파싱
  ▼
UserService.register_user()
  │
  │ 2. 이메일 중복 체크
  │ 3. UserRepository.create() 호출
  ▼
UserRepository.create()
  │
  │ 4. User 모델 생성
  │ 5. DB에 저장
  │ 6. User 객체 반환
  ▼
UserService (계속)
  │
  │ 7. create_access_token() - JWT 생성
  │ 8. 응답 데이터 구성
  ▼
auth.py (계속)
  │
  │ 9. JSON 응답 반환
  ▼
Client
  { "user": { "email": "...", "username": "...", "token": "JWT..." } }
```

### 2.2 로그인 (POST /users/login)

```
Client
  │
  │ POST /users/login
  │ { "user": { "email": "...", "password": "..." } }
  ▼
auth.py: login()
  │
  │ 1. UserLoginRequest 파싱
  ▼
UserService.login_user()
  │
  │ 2. UserRepository.get_by_email() - 이메일로 유저 조회
  │ 3. 비밀번호 검증 (현재는 평문 비교)
  │ 4. create_access_token() - JWT 생성
  │ 5. 응답 데이터 구성
  ▼
auth.py (계속)
  │
  │ 6. JSON 응답 반환
  ▼
Client
  { "user": { "email": "...", "username": "...", "token": "JWT..." } }
```

---

## 3. 인증된 요청 흐름 (현재 유저 조회)

### GET /user (현재 유저 정보)

```
Client
  │
  │ GET /user
  │ Headers: { "Authorization": "Token JWT..." }
  ▼
FastAPI Dependency 실행
  │
  │ get_current_user() 의존성 함수
  ▼
dependencies.py: get_current_user()
  │
  │ 1. Authorization 헤더 추출
  │ 2. "Token " 제거하여 JWT 추출
  ▼
security.py: verify_token()
  │
  │ 3. JWT 검증 (서명, 만료 시간)
  │ 4. payload 추출 { "user_id": ..., "username": ... }
  ▼
dependencies.py (계속)
  │
  │ 5. UserRepository.get_by_id() - user_id로 DB 조회
  │ 6. User 객체 반환
  ▼
auth.py: get_current_user_endpoint()
  │
  │ current_user 파라미터로 User 객체 주입됨
  ▼
UserService.get_user_profile()
  │
  │ 7. 새로운 JWT 토큰 생성
  │ 8. 응답 데이터 구성
  ▼
Client
  { "user": { "email": "...", "username": "...", "token": "NEW_JWT..." } }
```

---

## 4. 유저 정보 수정 흐름

### PUT /user

```
Client
  │
  │ PUT /user
  │ Headers: { "Authorization": "Token JWT..." }
  │ Body: { "user": { "username": "new", "bio": "..." } }
  ▼
get_current_user() 의존성
  │ (위의 3번 흐름과 동일)
  │
  │ current_user 객체 주입
  ▼
auth.py: update_user()
  │
  │ 1. UserUpdateRequest 파싱
  ▼
UserService.update_user()
  │
  │ 2. current_user 객체의 필드 수정
  │    - email, username, password, bio, image
  │ 3. session.commit() - DB 업데이트
  │ 4. 새로운 JWT 토큰 생성
  │ 5. 응답 데이터 구성
  ▼
Client
  { "user": { "email": "...", "username": "new", "token": "JWT..." } }
```

---

## 5. 프로필 조회 흐름

### GET /profiles/{username}

```
Client
  │
  │ GET /profiles/johndoe
  │ (인증 선택사항)
  ▼
profile.py: get_profile()
  │
  │ 1. username 파라미터 추출
  ▼
ProfileService.get_profile()
  │
  │ 2. UserRepository.get_by_username() - username으로 조회
  │ 3. 유저 없으면 404 HTTPException
  │ 4. following 상태는 False (현재 구현)
  │ 5. 응답 데이터 구성
  ▼
Client
  { 
    "profile": { 
      "username": "johndoe", 
      "bio": "...", 
      "image": "...",
      "following": false 
    } 
  }
```

---

## 6. 팔로우/언팔로우 흐름

### 6.1 POST /profiles/{username}/follow

```
Client
  │
  │ POST /profiles/johndoe/follow
  │ Headers: { "Authorization": "Token JWT..." }
  ▼
get_current_user() 의존성
  │
  │ current_user 객체 주입
  ▼
profile.py: follow_user()
  │
  │ 1. username 파라미터 추출
  ▼
ProfileService.follow_user()
  │
  │ 2. UserRepository.get_by_username() - 팔로우 대상 조회
  │ 3. 대상 없으면 404 HTTPException
  │ 4. 자기 자신이면 422 HTTPException
  │ 5. FollowRepository.is_following() - 이미 팔로우 중인지 체크
  │ 6. 팔로우 중이 아니면 FollowRepository.create()
  ▼
FollowRepository.create()
  │
  │ 7. Follow 모델 생성 (follower_id, followee_id)
  │ 8. DB에 저장
  ▼
ProfileService (계속)
  │
  │ 9. 응답 데이터 구성 (following=true)
  ▼
Client
  { 
    "profile": { 
      "username": "johndoe",
      "bio": "...",
      "image": "...",
      "following": true 
    } 
  }
```

### 6.2 DELETE /profiles/{username}/follow

```
Client
  │
  │ DELETE /profiles/johndoe/follow
  │ Headers: { "Authorization": "Token JWT..." }
  ▼
get_current_user() 의존성
  │
  │ current_user 객체 주입
  ▼
profile.py: unfollow_user()
  │
  │ 1. username 파라미터 추출
  ▼
ProfileService.unfollow_user()
  │
  │ 2. UserRepository.get_by_username() - 언팔로우 대상 조회
  │ 3. 대상 없으면 404 HTTPException
  │ 4. FollowRepository.delete() 호출
  ▼
FollowRepository.delete()
  │
  │ 5. Follow 관계 조회 (follower_id, followee_id)
  │ 6. 존재하면 삭제, 없으면 그냥 반환
  ▼
ProfileService (계속)
  │
  │ 7. 응답 데이터 구성 (following=false)
  ▼
Client
  { 
    "profile": { 
      "username": "johndoe",
      "bio": "...",
      "image": "...",
      "following": false 
    } 
  }
```

---

## 7. 데이터베이스 스키마

### User 테이블
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    email VARCHAR UNIQUE NOT NULL,
    username VARCHAR UNIQUE NOT NULL,
    hashed_password VARCHAR NOT NULL,
    bio VARCHAR NULL,
    image VARCHAR NULL
);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
```

### Follow 테이블
```sql
CREATE TABLE follows (
    id INTEGER PRIMARY KEY,
    follower_id INTEGER NOT NULL,
    followee_id INTEGER NOT NULL,
    FOREIGN KEY (follower_id) REFERENCES users(id),
    FOREIGN KEY (followee_id) REFERENCES users(id)
);
```

---

## 8. JWT 토큰 구조

```json
{
  "user_id": 1,
  "username": "johndoe",
  "exp": 1701234567
}
```

- SECRET_KEY로 서명됨 (HS256 알고리즘)
- 만료 시간: 7일
- 검증 실패 시 401 Unauthorized

---

## 9. 핵심 설계 원칙

### 레이어 간 데이터 흐름
```
Request → API → Service → Repository → Model → Database
Response ← API ← Service ← Repository ← Model ← Database
```

### 각 레이어의 책임
- **API Layer**: HTTP 프로토콜 처리
- **Service Layer**: 비즈니스 규칙, 여러 Repository 조합
- **Repository Layer**: 단순 CRUD 연산
- **Model Layer**: 데이터 구조 정의

### 의존성 방향
```
API → Service → Repository → Model
     ↓
   Core (dependencies, security, database)
```

상위 레이어는 하위 레이어를 의존하지만, 하위는 상위를 모름 (의존성 역전 원칙)

---

## 10. 현재 구현된 엔드포인트

### Auth
- `POST /users` - 회원가입
- `POST /users/login` - 로그인
- `GET /user` - 현재 유저 정보 (인증 필요)
- `PUT /user` - 유저 정보 수정 (인증 필요)

### Profile
- `GET /profiles/{username}` - 프로필 조회 (공개)
- `POST /profiles/{username}/follow` - 팔로우 (인증 필요)
- `DELETE /profiles/{username}/follow` - 언팔로우 (인증 필요)

---

이것이 현재 시스템의 완전한 흐름도입니다.