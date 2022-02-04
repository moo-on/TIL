### 가상환경세팅

```bash
python3 -m venv sandbox
source sandbox/bin/activate # 가상환경 진입
pip install wheel # airflow 종속성 모듈
pip3 install apache-airflow==2.1.0 --constraint https://gist.githubusercontent.com/marclamberti/742efaef5b2d94f44666b0aec020be7c/raw/21c88601337250b6fd93f1adceb55282fb07b7ed/constraint.txt
airflow db init # 초기 db 설정
airflow webserver, scheduler # 포트에 서비스 실행

# 기타 명령어는 -h 옵션으로 보기
```

### Airflow란?

- Orchestrator Task : 각 태스크들을 적절한 타이밍에 실행 시킬수 있다.
- 확장성이 좋다.
    - 필요한 만큼 인스턴스를 늘릴 수 있다.
- UI가 좋다

### 구성요소

- Web Server
    - Flask와 Gunicon으로 사용자와 상호작용 할수있는 웹 페이지가 제공된다.
- Scheduler
    - Demon 방식의 프로세스로 스케줄러의 흐름을 담당한다.
- Meta Store
    - airflow와 관ㅓ련된 데이터 및 작업 이력 등이 저장된다.
    - database가 sqlalchemy랑 호환이되면 recall도 가능하다.
- Executor
    - 작업이 실행되는 방법을 정의한다. 쿠버네티스 or 셀러리 or 로컬에서
- Worker
    - 작업을 실제로 이행하는 프로세스

### Operator

- **Action Operator** : 함수나 명령 실행
    - bash Operator
    - python Operator
- **Transfer Operator** : 데이터 간의 혹은 소스 간의 이동
- **Sensor Operator** : 특정 조건이 만족해야만 다음 작업으로 이동

### 동작방식

- **단일 노드 아키텍처**
    - 테스트를 하거나 작업이 적을 경우 하나의 노드에 Web Server, Scheduler, Executor, Queue, Meta store 전부 하나의 노드에서 실행
- **단일 노드 아키텍처 프로세스**
    1. Dag가 들어오면 Web Server & Scheduler가 읽어들인다.
    2. 트리거될 준비가되면 Meta Store에 DagRun 객체가 생성된다.
    3. 실행해야할 첫번째 작업이 예약된다.
    4. 첫번째 작업을 실행할 task instance가 만들어진다.
    5. 만들어진 instance는 스케줄러에 의해 executor에게 보낸후 실행된다.
    6. Executor에 의해 실행된 인스턴스는Meta Store의 instance의 정보를 업데이트 한다.
    7. 마지막으로 스케줄러는 작업이 완료되었는지 체크한다.
    8.  완료되었으면 Web server에서 UI가 업데이트 된다. 

**다중 노드 아키텍처**

- Node1(Web server, Scheduler, Executor) → master node
- Node2(Meta store, Queue) 이외에 Worker Node를 임의로 생성해 작업할 수 있다.
- scheduler → executor || meta store 통신하여 작업 실행
- web server는 meta store의 정보를 가져온다.
- External executor가 들어있는 Queue에서 작업을 하나씩 꺼내서 worker node에게 배부1

### Airflow 웹서버 기능

- **tree view**
    - DAG의 히스토리 까지 조회 가능
- **graph view**
    - 현재 다이어그램만 조회 가능하다.
    - 노드 누를 시 상세 정보 확인 가능
        - clear : 다시 실행하거나 재실행 할 시
        - mark failed, mark success : 강제로 해당 task의 상태를 변경해서 플로우에 어떤 영향을 끼치는지 체크
- **gantt view**
    - 병목 현상 확인할 시 유용하다.
