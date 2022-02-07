# Scale Up - section4

### scale up & **parallel** **를 위한 기본 구성 방법**

**parallel 조건**

- 다중 접속이 가능한 타 DB를 이용한다.
    - sqlite는 동시에 여러 권한을 허용하지 않는다. 그렇기에 하나의 작업이 끝나야만 해당 DB를 사용할 수 있다.
- SequentialExecutor가 아닌 LocalExcutor를 사용한다.
- t1 >> [t2, 53]

**postgresql setting**

```bash
sudo apt install postgresql
sudo -u postgres psql
pip install 'apache-airflow[postgres]'

# airflow.cfg 파일 수정해서 metasotre을 sqlite에서 postgresql로 변경
sql_alchemy_conn = postgresql+psycopg2://postgres:postgres@localhost/postgres

# 새로연결한 db 기본파일 생성
airflow db init
# 기존 aiflow.db에서 다른 곳으로 연결됌
airflow db check 
# 유저 생성
airflow users create -u admin -p admin -r Admin -f admin -l admin -e admin@airflow.com

```

**기본적인 구조**

- 트리거가 걸리면 Queue 안에 모든 태스크들이 들어간다.
- Queue에서 worker에게 작업이 분산된다.
    - local excutor의 경우 단일 노드에서 작업을 해야하므로 확장성에 문제가 있다.
    - celery excutor와 redis를 조합해서 원하는만큼 worker를 생성해 확장성을 높일 수 있다.

**celeryExcutor & redis settings**

```bash
---install-----
pip install 'apache-airflow[celery]'
sudo apt install redis-server
pip install 'apache-airflow[redis]'  

----setting-------
sudo nano /etc/redis/redis.conf
	supervised no -> supervised systemd
systemctl restart redis.service

----airflow.cfg----
excutor = CeleryExecutor

# redis 연결될 url
broker_url = redis://localhost:6379/0

# sql_alchemy_conn과 같은 저장소 사용
result_backend = postgresql+psycopg2://postgres:postgres@localhost/postgres

airflow celery flower
```

**parallelism**

- instance에서 병렬로 처리할 수 있는 작업 수

**dag_concurrency**

- 동시에 돌아갈 수 있는 dag 수
- config파일 외 dag에서 직접 처리 가능하다.

**max_active_runs_per_dag**

- 최대 활성 가능 dag 수
- 최대 활성 가능 수를 넘으면 start 자체가 안된다.
