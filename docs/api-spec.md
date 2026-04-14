# API 명세

<br>

## 1. 인증 / 로그인

<img src="api01.png" alt="api01" width="430" />

* `{provider}`: `google`, `kakao`, `naver`

<br>
<br>

## 2. 게시물 목록

<img src="api02.png" alt="api02" width="430" />

* 검색, 정렬, 필터, 무한 스크롤 → Query Parameter 처리

<br>
<br>

## 3. 게시물 관리

<img src="api03.png" alt="api03" width="430" />

* 게시물 삭제 → 상태값 변경 (Soft Delete)

<br>
<br>

## 4. 투표 / 신고

<img src="api04.png" alt="api04" width="430" />

<br>
<br>

## 5. 마이페이지

<img src="api05.png" alt="api05" width="430" />

* 회원 탈퇴 → 상태값 변경

<br>
<br>

## 6. 채팅

<img src="api06-1.png" alt="api06-1" width="490" />

* 채팅방 → 게시물 기준 생성

<br>

### WebSocket

<img src="api06-2.png" alt="api06-2" width="430" />

<br>
<br>

## 7. 관리자

<img src="api07.png" alt="api07" width="430" />

<br>
<br>

## 8. 상태 정의

### 유저 상태

<img src="status01.png" alt="status01" width="380" />

### 게시물 상태

<img src="status02.png" alt="status02" width="380" />

### 투표 상태

<img src="status03.png" alt="status03" width="380" />

### 스팸 필터 유형

<img src="status04.png" alt="status04" width="380" />
