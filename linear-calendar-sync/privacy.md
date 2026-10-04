---
title: linear-calendar-sync 개인정보처리방침
lang: ko-KR
---

# linear-calendar-sync 개인정보처리방침

- 시행일: 2026-10-03
- 기준: linear-claude-workflow 패키지 0.1.9에 든 linear-calendar-sync
- 홈페이지: [https://chansikkim86.github.io/linear-calendar-sync/](https://chansikkim86.github.io/linear-calendar-sync/)

이 방침은 linear-calendar-sync(아래 "이 도구")가 구글 사용자 데이터에 어떻게 접근하고, 그 데이터를 어떻게 사용·저장·공유하는지 밝힌다. 이 도구는 개발자 본인이 자신의 PC에서만 쓰는 개인용 도구이고 일반에 제공하지 않는다. 이 도구를 위해 운영하는 서버는 없다.

## 요청하는 구글 권한

구글 인증에서 `https://www.googleapis.com/auth/calendar.app.created` 권한 하나만 요청한다. 이 권한으로는 이 도구가 만든 캘린더와 그 event만 다룰 수 있고, 다른 캘린더는 읽지도 고치지도 못한다. 이름이나 이메일 주소 같은 구글 계정 정보는 요청하지 않는다.

## 접근

- 인증: 사용자가 브라우저에서 허용하면 구글에서 refresh token과 access token을 받는다. 인증 응답은 이 PC의 `127.0.0.1`에만 연 수신기가 받고, 수신기는 요청 하나를 받으면 닫힌다.
- 캘린더: setup은 기록해 둔 "Linear_Issue"·"Linear_Project" 캘린더가 각각 있는지 확인하고, 없는 쪽을 새로 만든다.
- event 읽기: 실행마다 두 캘린더의 event 목록을 하나씩 읽는다.
- event 쓰기: 대상 Linear 항목마다 그 종류의 캘린더(이슈는 "Linear_Issue", 프로젝트는 "Linear_Project")에 event를 하나 등록하고, Linear 쪽 값이 바뀌면 바뀐 필드(제목, 설명, 날짜)만 고친다. event에는 이슈 제목이나 프로젝트 이름, Linear의 설명 원문 전체와 그 항목의 Linear URL, 날짜, 화면에 보이지 않는 꼬리표(Linear 고유 id)가 들어간다.
- 지우지 않음: 실행과 setup은 event와 캘린더를 지우지 않는다. 꼬리표가 없는 event(사람이 두 캘린더에 직접 만든 event)는 건드리지 않는다.

## 사용

- event 목록은 이 도구가 만든 event가 그 캘린더에 아직 있는지 확인하고, 로컬 기록 없이 남은 이 도구의 event를 찾아 다시 맡는 데 쓴다.
- 토큰은 구글에서 access token을 받고 Calendar API를 부르는 데만 쓴다.
- 이 도구는 코드로만 동작하며 LLM을 쓰지 않는다.
- 구글 사용자 데이터는 이 방침에 적은 용도로만 쓴다.

## 저장

이 PC에만 저장한다. access token은 메모리에서만 쓰고 저장하지 않는다.

| 무엇 | 내용 |
|---|---|
| refresh token | 소유자만 읽고 쓸 수 있는 파일(권한 600)에 둔다 |
| 캘린더 기록 | "Linear_Issue"·"Linear_Project" 캘린더의 ID |
| 항목 기록 | Linear 항목마다 event ID와 마지막으로 event에 쓴 값. 제목과 설명은 sha256 값만 남긴다 |
| 로그 | 실행의 시작과 끝, 항목마다 식별자(이슈 식별자, 프로젝트 `slugId`, event ID)와 한 일, 실패 원인. 구글 쪽 실패는 HTTP 상태와 오류 이름만 적는다. 제목, 설명, 토큰, 인증 코드는 적지 않는다. 로그는 회전하거나 지우지 않는다 |

## 공유

구글에서 받은 데이터를 제3자에게 보내지 않는다. Linear는 읽기만 하고 Linear에는 아무것도 쓰지 않는다.

## Limited Use

linear-calendar-sync가 Google API에서 받은 정보를 사용하는 방식은 Limited Use 요건을 포함한 [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy)를 따른다.

영어 원문:

> linear-calendar-sync's use of information received from Google APIs will adhere to [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy), including the Limited Use requirements.

## 접근 취소와 삭제

1. 구글 계정에서 접근 취소: [Google 계정의 서드 파티 연결 페이지](https://myaccount.google.com/linkedapps)에서 "Google 계정 액세스"를 선택하고 이 도구를 고른 뒤 "세부정보 보기" → "액세스 권한 삭제" → "확인"을 누른다. 구글 도움말 「[Google 계정과 서드 파티 간의 연결 관리하기](https://support.google.com/accounts/answer/13533235?hl=ko)」에 같은 절차가 있다. 취소한 뒤의 실행은 event를 만들지 않고 종료 코드 1로 끝난다.
2. 이 PC의 파일 삭제: 위 "저장"의 refresh token, 캘린더 기록, 항목 기록, 로그 파일을 지운다. 이 도구에는 이 파일들을 지우는 명령이 없으므로 사람이 직접 지운다.
3. 캘린더 삭제: 이 도구는 event와 캘린더를 지우지 않는다. 캘린더와 그 event를 없애려면 컴퓨터에서 Google Calendar를 열고 오른쪽 위 설정 → 설정 → 왼쪽 열에서 "Linear_Issue"나 "Linear_Project" 캘린더(이름을 바꿨다면 바꾼 이름. 0.1.8 이하가 만든 "Linear" 캘린더도 같다) → 캘린더 삭제 → 삭제 → 완전히 삭제를 누른다. 구글 도움말 「[캘린더 삭제 또는 구독 취소](https://support.google.com/calendar/answer/37188?hl=ko)」의 "보조 캘린더 삭제"에 같은 절차가 있다.
