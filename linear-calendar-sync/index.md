---
title: linear-calendar-sync
lang: ko-KR
---

# linear-calendar-sync

linear-calendar-sync는 Linear 이슈의 due date는 구글 캘린더의 전용 캘린더 "Linear_Issue"에, 프로젝트의 기간은 "Linear_Project"에 하루 종일 event로 옮기고, Linear 쪽이 바뀌면 event를 맞춰 고치는 명령줄 도구다.

개인정보처리방침: [https://chansikkim86.github.io/linear-calendar-sync/privacy.html](https://chansikkim86.github.io/linear-calendar-sync/privacy.html)

## 개인용 도구

개발자 본인이 자신의 PC에서만 쓰는 개인용 도구다. 일반에 제공하지 않는다. 이 도구를 위해 운영하는 서버는 없다.

## 하는 일

- 처음 한 번 `setup`으로 구글 인증을 하고 "Linear_Issue"·"Linear_Project" 두 캘린더를 만든다. 그 뒤로는 Windows 작업 스케줄러가 주기적으로 실행한다.
- 실행마다 두 캘린더의 event 목록을 읽고, 대상 항목마다 그 종류의 캘린더(이슈는 "Linear_Issue", 프로젝트는 "Linear_Project")에 event를 등록하거나 Linear에서 바뀐 필드(제목, 설명, 날짜)만 고친다.
- 동기화는 Linear → 구글 한 방향이다. Linear는 읽기만 하고 아무것도 쓰지 않는다.
- event와 캘린더를 지우지 않는다. 사람이 두 캘린더에 직접 만든 event는 건드리지 않는다.
- 코드로만 동작하며 LLM을 쓰지 않는다.

| | 이슈 | 프로젝트 |
|---|---|---|
| 대상 | 상태가 `Todo`, `In Progress`, `In Review` 가운데 하나이고 due date가 있는 이슈 | start date와 target date가 둘 다 있는 프로젝트 |
| event 제목 | 이슈 제목 | 프로젝트 이름 |
| event 설명 | 이슈 설명(Markdown 원문)과 그 이슈의 Linear URL | 프로젝트 본문(Markdown 원문)과 그 프로젝트의 Linear URL |
| 시작일 | `on-date` 라벨이 있으면 due date. 없으면 due date를 입력한 날과 due date 가운데 이른 날 | start date와 target date 가운데 이른 날 |
| 종료일 | due date | target date |

보관했거나 휴지통에 있는 이슈와 프로젝트는 대상이 아니다. event는 하루 종일 event이고 바쁨/한가함은 "한가함"이다.

## 구글 권한

구글 권한은 `https://www.googleapis.com/auth/calendar.app.created` 하나만 요청한다. 이 권한으로는 이 도구가 만든 캘린더와 그 event만 다룰 수 있고, 다른 캘린더는 읽지도 고치지도 못한다. 구글 사용자 데이터를 어떻게 접근·사용·저장·공유하는지는 [개인정보처리방침](https://chansikkim86.github.io/linear-calendar-sync/privacy.html)에 적었다.
