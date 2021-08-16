## Node.js 교과서
* express 를 이용한 SNS 서비스 만들기

### DB
* 클래스 모델 기반 데이터 베이스 생성 `npx sequelize db:create`
* MySql 정보 :: config/config.json

### TEST
* 테스트를 위해 `npm i -D jest` 사용    
* 통합테스트 위해 `npm i -D supertest` 사용
  * config에서 test db설정을 따로 하고, 데이터베이스 생성을 위해 `npx sequelize db:create --env test` 입력

### 부하테스트
* artillery 설치 `npm i -D artillery`
* 명령실행 `npx artillery quick --count 100 -n 50 http://localhost:8081`
  * count : 가상의 사용자 수
  * n : 요청 횟수
* sample output
```
All virtual users finished
Summary report @ 22:29:34(+0900) 2021-08-16
  Scenarios launched:  100
  Scenarios completed: 100
  Requests completed:  5000
  Mean response/sec: 331.79
  Response time (msec):
    min: 9
    max: 693
    median: 226
    p95: 417
    p99: 463
  Scenario counts:
    0: 100 (100%)
  Codes:
    200: 5000
```
* loadtest.json 처럼 시나리오 작성하여 실행 가능
  * yaml 파일로 변경 :: `npx artillery convert loadtest.json`
  * 시나리오 실행 :: `npx artillery run loadtest.json`
  * 로컬에서 테스트 7,200 번의 요청을 견지디 못함. db 요청을 버티지 못하고 타임아웃발생
```
All virtual users finished
Summary report @ 22:38:14(+0900) 2021-08-16
  Scenarios launched:  1800
  Scenarios completed: 12
  Requests completed:  1358
  Mean response/sec: 38.32
  Response time (msec):
    min: 12
    max: 9652
    median: 2574
    p95: 6061.6
    p99: 9207.9
  Scenario counts:
    0: 1800 (100%)
  Codes:
    200: 1102
    302: 128
    404: 128
  Errors:
    ETIMEDOUT: 1916
```