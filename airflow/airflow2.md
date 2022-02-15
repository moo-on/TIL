airflow 버전2가 되면서 바뀐점

**아키텍처**

- 로드밸런스를 통해 여러개의 웹서버 중 하나에게 요청이 된다.
    - 데이터베이스 마스터 노드와 읽기 전용 노드를 통해 쉽게 구현
- 스케줄러도 Parallel하게 만들어 하나의 스케줄러에 에러가 발생해도 계속 작업을 수행해 나갈수 있다.
    - active-active모델 기반
- Dag Serialization : meta store를 이용한 직렬화를 통해 오버헤드 줄임
    - [https://airflow.apache.org/docs/apache-airflow/stable/dag-serialization.html](https://airflow.apache.org/docs/apache-airflow/stable/dag-serialization.html)
- Dag versioning이 meta store를 통해 지원
- Rest API를 통해 dag or connection등이 안정적
- Functional DAGs
    - convert functions to tasks so easy
    - pluggable XCom Storage Engine
- Keda를 이용해서 효율적으로 어쩌구 뭐라는지 모르겠다
