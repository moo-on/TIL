### 작업 스케줄링

**at**

- 단일성 작업 예약도구
- 시스템 데몬 atd로 프로세스 제어
- 백그라운드에서 동작, 제어 터미널X
- 설치
    - yum -y install at
    - systemctl start atd
- 실행예제
    1. at now +1min
    2. echo “test” > test.txt
    3. ctrl + d <EOT>
- 기준 시간이 될 수 있는 문자열 예제
    - at teatime tomorrow 다음날 16:00
    - at noon +4 days 4일 뒤 정오
    - at 5pm august 3 2022
- 기타 옵션
    - at [-l] 예약된 명령어 확인
    - atrm 2 예약된 명령어 삭제

**주기적인 작업 예약**

- 예약된 작업을 반복 실행
- crond 데몬이 작업을 제어(crontab 구성 파일 해석)
- **#vim /etc/crontab**
    - crontab 파일확인
    - ***** root echo “test” > test.txt  → root 사용자가 매 분 마다 해당 명령어 실행
        - 분 시 일 달 요일
        - 01*** → 매일 새벽 1시 마다 실행
        - * 현재시간기준으로실행
        - 0/3 1 * * * (간격실행) → 매일 오전 1시에 3분 마다 실행하여 2시가되면 실행 x
    - 일반 사용자는 crontab -e 에서 반복 작업 생성
- **#vim /etc/cron.d/[주제]**
    - creat remove와 같은 주제에 알맞는 반복작업을 실행
- 기타 옵션
    - crontab [-l]  반복 작업 예약 확인
    - crontab [-r]  일반 사용자는 모든 반복 작업을 제거
- **#vim /etc/anacrontab**
    - 시스템이 종료되었거나 절전 중이라서실행을 못했을 경우 시스템이 가동된 시점을 기준으로 예약 작업 실행한다.
    1. 기간(일)
        - @daily 는 정수 1과 동일, 매일 실행됨을 의미
        - @weekly 는 정수 7과 동일, 매주 실행됨을 의미
        - @monthly 는 매달 실행됨을 의미(달마다 일 수가 다르기 때문)
    2. 지연(분)
        - 시스템이 준비된 후 작업을 시작하기 전에 crond 데몬이 대기해야
        하는 시간
    3. 작업 식별자
        - 작업이 식별되는 고유 이름
    4. 명령
        - 실행할 명령
