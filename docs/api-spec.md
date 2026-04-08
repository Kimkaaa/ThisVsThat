# API 명세

<br>

## 1. 인증 / 로그인

| 메서드  | 엔드포인트                       | 설명            |
| ---- | --------------------------- | ------------- |
| GET  | `/auth/{provider}/login`    | OAuth 로그인 요청  |
| GET  | `/auth/{provider}/callback` | OAuth 콜백 처리   |
| POST | `/auth/signup`              | 회원가입          |
| GET  | `/auth/me`                  | 로그인 사용자 정보 조회 |

* `{provider}`: `google`, `kakao`, `naver`
* OAuth 로그인 흐름 → `login → callback → signup`
* 토큰 검증 → `/auth/me`

<br>

## 2. 게시물 목록

| 메서드 | 엔드포인트    | 설명        |
| --- | -------- | --------- |
| GET | `/posts` | 게시물 목록 조회 |

* 검색, 정렬, 필터, 무한 스크롤 → Query Parameter 처리

<br>

## 3. 게시물 관리

| 메서드   | 엔드포인트                         | 설명                             |
| ----- | ----------------------------- | ------------------------------ |
| GET   | `/posts/{postId}`             | 게시물 상세 조회                      |
| POST  | `/posts`                      | 게시물 작성                         |
| PATCH | `/posts/{postId}`             | 게시물 수정                         |
| PATCH | `/posts/{postId}/status`      | 게시물 삭제 처리 (`ACTIVE → DELETED`) |
| PATCH | `/posts/{postId}/vote-status` | 투표 상태 변경 (`ACTIVE ↔ FINISHED`) |

* 게시물 삭제 → Soft Delete 방식 사용

<br>

## 4. 투표 / 신고

| 메서드  | 엔드포인트                         | 설명           |
| ---- | ----------------------------- | ------------ |
| POST | `/posts/{postId}/votes`       | 게시물 투표       |
| GET  | `/posts/{postId}/vote-result` | 게시물 투표 결과 조회 |
| POST | `/posts/{postId}/reports`     | 게시물 신고       |

* 투표 결과 → 집계 데이터 반환

<br>

## 5. 마이페이지

| 메서드   | 엔드포인트                 | 설명                              |
| ----- | --------------------- | ------------------------------- |
| GET   | `/users/me/dashboard` | 내 정보, 게시물, 투표, 채팅방 통합 조회        |
| PATCH | `/users/me`           | 내 정보 수정                         |
| PATCH | `/users/me/status`    | 회원 탈퇴 처리 (`ACTIVE → WITHDRAWN`) |

* 마이페이지 → 사용자 정보, 게시물, 투표, 채팅방 통합 조회
* 회원 탈퇴 → 상태값 변경

<br>

## 6. 채팅

| 메서드  | 엔드포인트                          | 설명                 |
| ---- | ------------------------------ | ------------------ |
| GET  | `/users/me/chat-profile`       | 채팅 프로필 조회          |
| POST | `/users/me/chat-profile/image` | 채팅 프로필 이미지 업로드     |
| GET  | `/chat-rooms/{postId}`         | 채팅방 입장 및 초기 데이터 조회 |
| POST | `/spam-filters/check`          | 메시지 전송 전 금지 키워드 검사 |

* 채팅방 → 게시물 기준 생성
* 채팅 입장 → 이전 메시지 + 초기 데이터 조회
* 채팅 프로필 → 세션 기반 임시 데이터

<br>

### WebSocket

| 방식        | 엔드포인트                       | 설명        |
| --------- | --------------------------- | --------- |
| WebSocket | `/ws`                       | 웹소켓 연결    |
| SUB       | `/sub/chatroom/{postId}`    | 채팅 메시지 구독 |
| PUB       | `/pub/join/{postId}`        | 채팅방 입장    |
| PUB       | `/pub/leave/{postId}`       | 채팅방 퇴장    |
| PUB       | `/pub/sendMessage/{postId}` | 메시지 전송    |

* 실시간 메시지 → WebSocket(STOMP) 기반 처리

<br>

## 7. 관리자

| 메서드    | 엔드포인트                                  | 설명                 |
| ------ | -------------------------------------- | ------------------ |
| GET    | `/admin/dashboard`                     | 관리자 데이터 통합 조회      |
| POST   | `/admin/spam-filters`                  | 금지 키워드 추가          |
| DELETE | `/admin/spam-filters`                  | 금지 키워드 삭제          |
| PATCH  | `/admin/posts/status`                  | 게시물 상태 일괄 변경       |
| PATCH  | `/admin/users/status`                  | 사용자 상태 일괄 변경       |
| GET    | `/admin/users/{userId}/reported-posts` | 특정 유저 신고/삭제 게시물 조회 |

* 관리자 → 주요 관리 데이터 통합 조회
* 상태 변경 → 일괄 처리

<br>

## 8. 상태 정의

### 유저 상태

| 상태 | 값         | 설명     |
| -- | --------- | ------ |
| 활성 | ACTIVE    | 정상 회원  |
| 탈퇴 | WITHDRAWN | 탈퇴 처리  |
| 차단 | BANNED    | 관리자 차단 |

<br>

### 게시물 상태

| 상태   | 값       | 설명    |
| ---- | ------- | ----- |
| 정상   | ACTIVE  | 정상 노출 |
| 블라인드 | BLINDED | 신고 누적 |
| 삭제   | DELETED | 논리 삭제 |

<br>

### 투표 상태

| 상태 | 값        | 설명      |
| -- | -------- | ------- |
| 진행 | ACTIVE   | 투표 진행 중 |
| 종료 | FINISHED | 투표 종료   |

<br>

### 스팸 필터 유형

| 유형  | 값       | 설명     |
| --- | ------- | ------ |
| 단어  | WORD    | 금지 단어  |
| 패턴  | PATTERN | 특정 패턴  |
| URL | URL     | 특정 URL |
