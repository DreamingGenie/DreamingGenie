<div align="center">

# 전진 | Backend

동시 편집 충돌을 막는 락 설계와 비용 상한을 넘으면 멈추는 데이터 수집기를 만들었고,<br/>
6명 팀 서비스의 배포, 관측, 백업을 맡았습니다.

SSAFY 15기 Java 트랙, 2026 하반기 신입 백엔드 지원 중

**[포트폴리오: dreaminggenie.github.io](https://dreaminggenie.github.io)**

</div>

## 프로젝트

### [BEEZ](https://github.com/S15A108/BEEZ), 6명 팀, 2026.10 ~ 11

팀원마다 AI 코딩 에이전트로 개발해도 모두 같은 승인 설계를 기준으로 일하게 돕는 협업 서비스

- **맡은 일** 서버 운영(PR 필수 검사, dev 자동 배포, 관측, 백업), 계정과 팀 API, 에이전트가 들어오는 MCP 진입점. 팀원 PR 103개 리뷰
- **대표 사례** 시간 제한에 끊기던 AI 문서 검토를 요청 밖 워커로 옮겼습니다

`Java 21` `Spring Boot 4.1` `PostgreSQL` `MCP Java SDK` `GitHub Actions` `Jenkins`

### [CoMeetTool](https://github.com/DreamingGenie/Comeettool), 7명 팀, 2026.07 ~ 08 (27일), 교육기관 GitLab에서 이관

음성 회의를 기록과 요약하고 팀 공간과 일정을 함께 관리하는 협업 도구

- **맡은 일** 팀 합의로 PM 겸 통합 담당. main 머지 10건 전부(그중 배포 9건), 팀 공간과 일정 기능과 공통 응답 틀
- **대표 사례** develop 브랜치 머지 95건 중 83건을 검토하고 머지했습니다

`Java 21` `Spring Boot 4.1` `Spring Data JPA` `PostgreSQL`

### [TripCraft](https://github.com/DreamingGenie/TripCraft), 2인 캡스톤, 혼자 추가 개발, 2026.05 ~ 11

여러 명이 함께 짜는 국내 여행 일정 플래너

- **맡은 일** 동시 편집 충돌 방지, 실시간 협업, 커뮤니티 게시판
- **대표 사례** 낙관적 락이 못 잡는 일정 겹침을 여행 단위 락으로 막고, 실 DB 테스트로 찾은 격리수준 결함을 고쳤습니다

`Java 21` `Spring Boot 3.5` `MyBatis` `MySQL 8` `WebSocket/STOMP` `Vue 3`

### [Pickage](https://github.com/DreamingGenie/Pickage), 6명 팀, 2026.08 ~ 10, 운영 종료, 교육기관 GitLab에서 이관

npm 패키지 후보 최대 3개를 생태계 변화, 기능, 커뮤니티 세 관점으로 비교하는 서비스

- **맡은 일** 원본 데이터 수집과 생태계 지표 계산. 개인 결제 계정으로 500TB가 넘는 BigQuery 데이터를 다뤄야 해서, 상한을 넘으면 멈추는 수집기를 만들었습니다
- **대표 사례** BigQuery 236번 실행에서 예상 사용량과 청구 사용량 차이가 0.1%였습니다

`Python` `BigQuery` `GCS` `DuckDB` `MinIO`

### [블록 배틀](https://github.com/DreamingGenie/BlockBattle), 1명, 2026.11, 서버 내림

BEEZ의 개발 절차로 만들어 AWS에 배포하고 실사용자를 받은 블록 낙하 퍼즐 대전 게임

- **맡은 일** 실증 범위와 지표를 정하고, 배포 뒤 운영을 판단했습니다. 코드는 AI 코딩 에이전트 세션이 썼습니다
- **대표 사례** 빠른 정상 이용자를 잡지 않게 기준을 세워, 랭킹 1위를 차지한 콘솔 봇을 랭킹에서 뺐습니다

`Java 21` `Spring Boot 3.5` `MySQL 8.4` `Redis 7.4` `React 19` `k6`

## 개인 작업

### [tichu-trainer](https://github.com/DreamingGenie/tichu-trainer), [바로 해 보기](https://dreaminggenie.github.io/tichu-trainer/)

보드게임 티츄의 규칙을 직접 카드를 내 보며 익히는 학습 웹입니다. 규칙 판정 코드를 화면에서 떼어 내, 설치 없이 `node tests/run.js` 한 줄로 테스트 85개를 돌립니다.

`바닐라 JS(ES 모듈)` `빌드 없음` `외부 라이브러리 0`

### [ServerTimeClicker](https://github.com/DreamingGenie/ServerTimeClicker)

서버가 초 단위로만 알려 주는 시각을 밀리초 단위까지 좁혀, 정해 둔 위치를 순서대로 클릭하는 데스크톱 앱입니다. 초가 바뀌는 순간 그 앞뒤 요청의 중간 시점을 서버의 초 경계로 봅니다.

`Java 21` `JavaFX` `jpackage`

## 일하는 방식

- **충돌 기준 (TripCraft)**: 서버가 이동시간을 다시 계산해 저장할 때도 버전이 오르면, 다른 사람이 일정을 고치지 않았는데도 사용자에게 충돌이 뜹니다. 그래서 이 UPDATE는 버전을 건드리지 않게 따로 나눴습니다.
- **검증 범위 (TripCraft)**: 캡스톤 때는 동시 요청 테스트가 없었습니다. 나중에 실제 MySQL로 붙여 보니 겹치는 일정이 둘 다 저장돼, 격리수준을 고치고 그 테스트를 남겼습니다.
- **집계 재현 (Pickage)**: 같은 집계를 다시 돌리자 14건이 달랐습니다. 발행 시각이 같은 릴리스끼리 순서가 실행마다 바뀌는 것이 원인이어서, 정렬 규칙을 하나 더하고 문서 숫자를 고쳤습니다.
- **설정으로 막은 실수 (CoMeetTool, Pickage)**: CoMeetTool은 기본 브랜치를 `develop`으로 바꿨고, Pickage는 비밀번호가 든 `.env.*` 파일 전체를 저장소에서 막았습니다(`.example`만 허용).
- **복원까지 확인 (BEEZ)**: 매일 백업을 만든 뒤, 서버 백업을 받아 임시 컨테이너에 복원하고 문서 4건의 파일 해시가 DB에 저장된 값과 같은지 확인했습니다.
- **AI 활용**: 무엇을 만들지 정하고 설계하고 결과를 검증하는 일은 제가 하고, 구현은 AI와 함께 합니다.

## 기술

| 깊이 | 기술 |
|---|---|
| 설계 이유까지 설명할 수 있는 것 | Java 21, Spring Boot, Spring Data JPA, MyBatis, PostgreSQL, Python, BigQuery, Git 브랜치 운영 |
| 써 본 것 | MySQL, Spring Security, Vue 3, JavaScript, Docker, AWS EC2, Nginx, GitHub Actions, Jenkins, Prometheus, GCS, DuckDB, MinIO, Redis, k6 |

## 알고리즘

**solved.ac Platinum V, rating 1,600, 279문제, Class 5**

[solved.ac 프로필](https://solved.ac/profile/yusengha), 백준 풀이는 [BJ](https://github.com/DreamingGenie/BJ) 저장소에 모아 두었습니다.

## 이력

| 기간 | 소속 | 비고 |
|---|---|---|
| 2026.01 ~ | 삼성청년 SW AI 아카데미(SSAFY) 15기 | Java 트랙 |
| 2024.07 ~ 2024.12 | 파워오토로보틱스 | 백엔드와 데이터 현장실습(학교 현장실습 6개월) |

| 자격 | 취득 | 발급 기관 |
|---|---|---|
| 정보처리기사 | 2024.06 | 한국산업인력공단 |
| SQL 개발자(SQLD) | 2026.03 | 한국데이터산업진흥원 |
| 한국사능력검정 1급 | 2019.11 | 국사편찬위원회 |

## 연락

- **dreaminggenie@naver.com** (대표, 개발과 업무용)
- dreaminggenie0704@gmail.com
