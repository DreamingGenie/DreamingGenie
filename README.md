<div align="center">

# 전진 · Backend

여러 사람이 동시에 고쳐도 데이터가 어긋나지 않게 만들고,<br/>
돌릴 때마다 돈이 나가는 작업은 실수해도 멈추게 만듭니다.

SSAFY 15기 Java 트랙 · 2026 하반기 신입 백엔드 지원 중

</div>

---

## 일하는 방식

- **무엇을 충돌로 볼지부터 정하고 잠급니다.** 서버가 다시 계산한 값 때문에 사용자에게 충돌이 뜨지 않도록, 그 쿼리는 버전을 건드리지 않게 나눴습니다.
- **재 보지 않은 것은 개선했다고 쓰지 않습니다.** 적용만 하고 검증하지 못한 것은 그렇다고 적습니다.
- **숫자가 달라지면 이유를 찾아 고칩니다.** 다시 돌린 집계가 14건 달랐던 원인(정렬 동점)을 찾아 문서를 고치고 정렬 규칙을 더했습니다.
- **실수는 말보다 설정으로 막습니다.** 기본 브랜치를 `develop`으로 바꾸고, 비밀번호가 든 `.env.*` 파일은 통째로 막았습니다.
- **AI와 함께 작업합니다.** 무엇을 만들지, 설계, 결과 검증은 제가 하고 구현은 AI와 함께 합니다.

---

## Key Projects

### [TripCraft](https://github.com/DreamingGenie/TripCraft) — 여러 명이 함께 짜는 국내 여행 일정 플래너

`Java 21` `Spring Boot` `MyBatis` `MySQL` `Vue 3` `WebSocket/STOMP` `Docker`

2인 캡스톤으로 시작해 혼자 추가 개발했습니다. 동시 편집 충돌 방지와 실시간 협업을 맡았습니다.

- 같은 일정을 동시에 고치면 버전 번호로 충돌을 알립니다(낙관적 락).
- 같은 시간·같은 순서 자리에 일정이 동시에 들어가지 않도록 여행 단위로 잠급니다(`FOR UPDATE`).
- 변경 알림, 외부 API 호출, 파일 삭제는 저장이 끝난 뒤로 미뤘습니다.
- 한계: 실제 동시 요청으로는 아직 검증하지 못했습니다.

### [tichu-trainer](https://github.com/DreamingGenie/tichu-trainer) — 보드게임 티츄 규칙을 직접 해 보며 익히는 학습 웹

`바닐라 JS(ES 모듈)` `빌드 없음` `외부 라이브러리 0`

혼자 만든 저장소입니다. 설치 없이 `node tests/run.js` 한 줄로 테스트 85개를 돌려 볼 수 있습니다.

- 규칙 판정 코드는 화면 코드를 전혀 부르지 않아, 퀴즈·미니 게임판·샌드박스의 판정이 어긋나지 않습니다.
- 강의 본문에 적은 카드 이름이 실제 카드인지까지 테스트합니다.

### [ServerTimeClicker](https://github.com/DreamingGenie/ServerTimeClicker) — 서버 시각에 맞춰 순서대로 클릭하는 데스크톱 앱

`Java 21` `JavaFX` `jpackage`

초 단위로만 알려 주는 서버 시각을 밀리초 단위로 좁히는 것이 핵심입니다.

- 초가 바뀌는 순간, 그 앞뒤 요청의 중간 시점을 서버의 초 경계로 봅니다.
- 여러 번 클릭할 때는 처음 정한 목표 시각을 기준으로 계산해 오차가 쌓이지 않습니다.

### 코드가 비공개인 것

- **CoMeetTool** (7명, 27일) — AI 회의 도우미. 팀 합의로 PM 겸 통합 담당을 맡아 develop에 합친 115건 중 92건과 main 머지 16건 전부(그중 배포 9건)를 처리했고, 팀 공간·일정 기능과 공통 응답 틀을 만들었습니다.
- **Pickage** (6명, 진행 중) — npm 패키지 후보를 생태계 변화·기능·커뮤니티 세 관점으로 비교하는 서비스. 개인 결제 계정으로 80TB 가까운 BigQuery 데이터를 다뤄야 해서, 상한을 넘으면 스스로 멈추는 수집기를 만들었습니다. 236번 실행해 보니 예상과 실제 청구량 차이가 0.1%였습니다.

---

## 학습 기록

- **[jin-GuestBook](https://github.com/DreamingGenie/jin-GuestBook)** — 방명록 하나로 Docker·GitHub Actions·EC2를 처음 다뤄 본 7일(2025-12)입니다. 그때 막혔던 자리가 실패한 CI 로그까지 그대로 남아 있습니다.
- **[BJ](https://github.com/DreamingGenie/BJ)** — 백준 풀이 모음입니다.

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

**자격**

| 자격 | 취득 | 발급 기관 |
|---|---|---|
| 정보처리기사 | 2024.06 | 한국산업인력공단 |
| SQL 개발자(SQLD) | 2026.03 | 한국데이터산업진흥원 |
| 한국사능력검정 1급 | 2019.11 | 국사편찬위원회 |

---

## Contact

- **dreaminggenie@naver.com** (대표 · 개발·업무용)
- dreaminggenie0704@gmail.com
