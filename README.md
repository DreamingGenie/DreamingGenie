<div align="center">

# 전진 · Backend

여러 사람이 동시에 고쳐도 데이터가 어긋나지 않게 만들고,<br/>
돌릴 때마다 돈이 나가는 데이터 작업은 실수해도 멈추게 만든다.

SSAFY 15기 Java 트랙 · 2026 하반기 신입 백엔드 지원 중

</div>

---

## 일하는 방식

- **무엇을 충돌로 볼지부터 정하고 잠근다.** TripCraft에서 일정을 옮기면 서버가 이동시간을 다시 계산해 저장한다. 이때 버전 번호까지 올리면, 사용자는 아무와도 부딪히지 않았는데 다음 저장에서 충돌을 받는다. 그래서 이동시간을 고치는 쿼리는 버전을 건드리지 않게 따로 뺐다.
- **재 보지 않은 것은 개선했다고 쓰지 않는다.** 전후를 재지 않은 곳에는 "개선"을 쓰지 않고, 적용만 하고 검증하지 못한 것은 그렇다고 적는다. TripCraft의 잠금도 실제 동시 요청으로는 검증하지 못했다(아래).
- **숫자가 달라지면 이유를 찾아 고친다.** 같은 집계를 다시 돌렸더니 의존 패키지를 뺀 사례가 14건 달랐다. 발행 시각이 똑같은 릴리스가 있으면 정렬 순서가 실행마다 바뀌기 때문이었다. 문서에 적은 숫자를 커밋으로 고쳤고, 나중에 정렬 규칙을 더해 이 원인을 막았다.
- **실수는 말보다 설정으로 막는다.** 저장소 기본 브랜치를 `develop`으로 바꿔, MR이 배포 브랜치(`main`)로 잘못 가는 길을 없앴다. 비밀번호가 든 `.env` 파일은 이름을 하나씩 더해 막는 대신 `.env.*`를 전부 막고 예시 파일만 허용했다. 하나씩 더하면 한 번 빠뜨렸을 때 그대로 올라가기 때문이다.
- **AI와 함께 작업한다.** 공개 저장소 커밋 상당수에 `Co-Authored-By: Claude` 표시가 붙어 있다. 무엇을 만들지와 설계, 결과 검증은 내가 하고 구현은 AI와 함께 했다. 표시가 없는 커밋이라고 AI 없이 쓴 것은 아니다.

---

## Key Projects

### [TripCraft](https://github.com/DreamingGenie/TripCraft) — 여러 명이 함께 짜는 국내 여행 일정 플래너

`Java 21` `Spring Boot` `MyBatis` `MySQL` `Vue 3` `WebSocket/STOMP` `Docker`

지도, 대중교통 이동시간, 시간표를 한 화면에 두고 여러 명이 동시에 편집한다. 2인 캡스톤(2026-05 ~ 06)으로 시작해 한 달 남짓 혼자 더 개발했고, 나는 동시 편집 충돌 방지와 실시간 협업, 커뮤니티 게시판을 맡았다. 커밋은 353건 중 161건이 내 것이지만 코드가 절반이라는 뜻은 아니다. 백엔드 테스트와 문서는 대부분 내가, 화면은 대부분 팀원이 썼고, 백엔드 Java는 약 28%가 내 줄이다. 대중교통 이동시간 계산 기능도 팀원 몫이다.

- **같은 일정을 동시에 고치면 뒤에 저장한 쪽에 알린다.** 일정마다 버전 번호를 두고 "내가 읽은 버전 그대로인가"를 확인한다. 그사이 누가 먼저 고쳤으면 저장하지 않고 충돌(409)을 돌려준다.
- **같은 시간에 일정 두 개가 들어가는 건 여행 단위 잠금으로 막았다.** 두 사람이 겹치는 시간에 각자 새 일정을 넣으면, 일정을 한 건씩 보는 버전 검사로는 충돌이 보이지 않는다. 그래서 일정을 넣거나 옮기기 전에 그 여행 자체를 잠가(`SELECT … FOR UPDATE`), 한 사람씩 차례로 겹침 검사를 하게 했다. 같은 날짜에 동시에 넣은 일정이 같은 순서 자리를 받는 문제도 함께 막힌다.
- **느린 일은 저장이 끝난 뒤로 미뤘다.** 변경 알림은 저장된 순서대로 보내려고, 외부 API를 부르는 이동시간 계산은 잠금을 오래 붙잡지 않으려고, 이미지 파일 삭제는 저장이 취소됐을 때 파일만 먼저 사라지지 않게 하려고 저장 뒤로 뺐다. 이유는 코드 주석에 남겼다.
- **약점.** 이 잠금이 두 요청이 실제로 동시에 올 때 막아 주는지는 테스트로 확인하지 못했다. 테스트 14개 중 13개가 DB를 흉내 낸 가짜 객체(Mockito)로 돌고, 실제 DB로 돌리는 테스트는 내 환경에서 실행되지 않았다.

### [tichu-trainer](https://github.com/DreamingGenie/tichu-trainer) — 보드게임 티츄 규칙을 직접 해 보며 익히는 학습 웹

`바닐라 JS(ES 모듈)` `빌드 없음` `외부 라이브러리 0`

혼자 만든 33커밋짜리 저장소다. 2026-08-20 ~ 21 이틀에 대부분을 만들고, 한 달 뒤 디자인을 다듬었다. **설치할 것이 없어서, clone한 뒤 `node tools/serve.js 8000`으로 띄우고 `node tests/run.js`를 치면 84개 테스트가 바로 돈다.**

- 규칙을 판정하는 코드(`src/engine`, 11파일)는 화면·브라우저 코드를 한 번도 부르지 않는다. 퀴즈 채점, 미니 게임판, 조합 판정 샌드박스가 모두 같은 판정 함수를 쓰기 때문에 세 곳의 판정이 어긋날 수 없다.
- 룰북이 애매하게 남긴 부분은 하나로 정하고 근거를 코드에 적었다. 예를 들어 만능 카드(봉황)는 여러 조합에 쓸 수 있지만 가장 센 조합(폭탄)에는 못 쓴다.
- 강의 본문도 테스트한다. 본문에 적은 카드 이름이 실제로 있는 카드인지 검사한다. 손으로 쓴 글의 오타는 그 화면에 들어가 봐야만 드러나기 때문이다.

### [ServerTimeClicker](https://github.com/DreamingGenie/ServerTimeClicker) — 서버 시각에 맞춰 순서대로 클릭하는 데스크톱 앱

`Java 21` `JavaFX` `jpackage`

혼자 만든 9커밋, 1,374줄짜리 앱이다. 웹 서버는 응답 헤더(`Date`)로 현재 시각을 알려 주지만 초 단위라, 그대로 쓰면 최대 1초가 틀린다. 이 1초를 밀리초 단위로 얼마나 좁힐 수 있는지가 이 저장소의 핵심이다.

- 짧은 간격으로 계속 물어보다가 초가 바뀌는 순간을 잡고, 바뀌기 직전과 직후 요청의 중간 시점을 서버의 초 경계로 본다(`localBoundary = (previous.middleMillis() + current.middleMillis()) / 2`).
- 클릭 직전에는 남은 시간에 따라 기다리는 방식을 바꾼다. 많이 남았으면 쉬고(`sleep`), 거의 다 되면 CPU를 써서 계속 확인한다(`onSpinWait`).
- 여러 번 클릭할 때는 직전 클릭이 아니라 처음 정한 목표 시각을 기준으로 간격을 계산해, 오차가 쌓이지 않는다.
- 서버와 내 PC의 시간 차이가 "0"인 것과 "아직 모름"을 구분한다(`isSyncedForTarget()`). 모를 때도 0으로 두면, 맞춰지지 않은 내 PC 시간이 서버 시간인 것처럼 보이기 때문이다.

### 코드가 비공개인 것

- **CoMeetTool** — SSAFY 공통 프로젝트(7명, 27일). 음성 회의를 기록·요약하고 팀 공간과 일정을 관리하는 협업 도구다. 팀 합의로 PM과 통합 담당을 맡아, 팀원이 올린 코드를 검토하고 공동 개발 브랜치에 합쳤다. develop 브랜치에 합친 115건 중 92건을 내가 처리했고(그중 71건이 팀원 코드), 배포 브랜치(main)로 내보낸 16건은 전부 내가 했다. 내 코드 중 10건은 팀원 3명이 검토하고 합쳤다. 팀 공간·일정 기능(API 14개), 모든 API가 같은 모양으로 응답하게 하는 공통 틀, 로그인 토큰(JWT) 검사 필터를 처음 만들었다. *코드는 비공개(SSAFY GitLab).*
- **Pickage** — 오픈소스 패키지가 어떤 패키지로 갈아타졌는지 추적해 대체재를 찾는 서비스(6명, 진행 중). 나는 데이터 수집·집계를 맡았다. BigQuery는 쿼리가 읽은 양만큼 요금을 받는데, 결제는 내 개인 계정이었고 데이터셋은 80TB 가까이 됐다. 그래서 실행 전에 사용량을 미리 계산하고 정해 둔 상한을 넘으면 스스로 멈추는 수집기를 만들었다. 236번 돌려 보니 실제 청구량이 예상과 0.1% 차이였다. PC 한 대에서 분석용 DB(DuckDB)로 릴리스 약 4,700만 건을 집계했다. *코드는 비공개(SSAFY GitLab).*

---

## 학습 기록

- **[jin-GuestBook](https://github.com/DreamingGenie/jin-GuestBook)** — 방명록 하나로 Docker·GitHub Actions·EC2를 처음 다뤄 본 7일(2025-12). 지금이면 다르게 짤 곳이 많고, 그때 막혔던 자리가 실패한 CI 로그까지 저장소에 그대로 남아 있다.
- **[BJ](https://github.com/DreamingGenie/BJ)** — 백준 풀이 아카이브.

---

## Tech Stack

**Backend**

![Java](https://img.shields.io/badge/Java-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/Spring%20Data%20JPA-59666C?style=flat-square)
![MyBatis](https://img.shields.io/badge/MyBatis-C74634?style=flat-square)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)

**Data**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![BigQuery](https://img.shields.io/badge/BigQuery-669DF6?style=flat-square&logo=googlebigquery&logoColor=white)
![DuckDB](https://img.shields.io/badge/DuckDB-FFF000?style=flat-square&logo=duckdb&logoColor=black)
![MinIO](https://img.shields.io/badge/MinIO-C72E49?style=flat-square&logo=minio&logoColor=white)

**Frontend · 기타**

![Vue.js](https://img.shields.io/badge/Vue.js-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white)

---

## 알고리즘

**solved.ac Platinum V · rating 1,600 · 279문제 · Class 5**

[![solved.ac](https://mazassumnida.wtf/api/v2/generate_badge?boj=yusengha)](https://solved.ac/profile/yusengha)

---

## Experience

| 기간 | 소속 | 비고 |
|---|---|---|
| 2026.01 ~ | 삼성청년SW아카데미(SSAFY) 15기 | Java 트랙 |
| 2024.07 ~ 2024.12 | 파워오토로보틱스 | 백엔드·데이터 현장실습(학교 현장실습 6개월) |

자격 · SQLD · 정보처리기사

---

## Contact

- **yusengha@naver.com** (대표 · GitHub 계정 연결 주소)
- dreaminggenie0704@gmail.com
