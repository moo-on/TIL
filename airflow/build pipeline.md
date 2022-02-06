# 파이프라인 구축 - section3

### operator란?

- 파이프 라인 안에서 단일의 Task를 정의해야한다.

**Operator 종류**

- Action Operator
- Transfer Operator
- Sensors

**파이프라인 순서**

1. 테이블 생성 → sqlite
2. http sensor 통해 API가 사용가능한지 확인
3. http operator를 통해 데이터를 추출 
4. python operator를 통해 processing
5. 가공된 정보를 테이블에 저장

**Start guide**

```bash
home/airflow/airflow # 접속
mkdir dags # 폴더 생성
./dags/user_processing.py # py파일생성
```

**SQLite** **Connection 세팅**

```bash
source sandbox/bin/activate
pip install apache-airflow-providers-sqlite  # docs에서 가져온다.

from airflow.providers.sqlite.operators.sqlite import SqliteOperator
-> user_processing.py

-----WEB--------
# 상단 admin tab -> db_sqlite 생성
# Conn id : user_processing.py에서 사용될 id
# Host : /home/airflow/airflow/airflow.db

```

**user_processing.py**

```python
from airflow.models import DAG
from airflow.providers.sqlite.operators.sqlite import SqliteOperator
from airflow.providers.http.sensors.http import HttpSensor

from datetime import datetime

default_args = {
    'start_date': datetime(2020, 1, 1)
}

with DAG('user_processing', 
    schedule_interval='@daily', 
    start_date=datetime(2020, 1, 1),  # default_args=default_args
    catchup=False) as dag:
```

**작업 테스트 및 검증**

```bash
# task를 생성했으면, 테스트해봐야한다.

# tasks test python file/task_id/start_date
airflow tasks test user_processing creating_table 2020-01-01 # log에 success

sqlite3 airflow.db
# .tables command로 user tables 확인

```

**API 사용 여부를 파악하기 위한** **http sensor 사용 → user_processing.py**

```python
pip install apache-airflow-providers-http==2.0.0  # 가상환경

from airflow.providers.http.sensors.http import HttpSensor

	is_api_available = HttpSensor(
	    task_id='is_api_available',
	    http_conn_id='user_api',
	    endpoint='api/'
	)

----webserver connection-----------
'''
Conn Id : user_api
Conn Type : http
Description	: API for getting users
Host : https://randomuser.me/
'''

airflow tasks test user_processing is_api_available 2020-01-01
```

**api 사용하여 데이터 불러오기 → user_processing.py**

```python
from airflow.providers.http.operators.http import SimpleHttpOperator
import json

	extracting_user = SimpleHttpOperator(
	        task_id='extracting_user',
	        http_conn_id='user_api',
	        endpoint='api/',
	        method='GET',
	        response_filter=lambda response: json.loads(response.text),
	        log_response=True
	    )
	
```

**python operator로 전처리 → user_processing.py**

```python
fromairflow.operators.python import PythonOperator  # 기본모듈이라 pipx
from pandas import json_normalize

def _processing_user(ti):
    users = ti.xcom_pull(task_ids=['extracting_user'])
    if not len(users) or 'results' not in users[0]:
        raise ValueEroor('User is empty')
    user = users[0]['results'][0]
    processed_user = json_normalize({
        'firstname' : user['name']['first'],
        'lastname' : user['name']['last'],
        'country' : user['location']['country'],
        'username' : user['login']['username'],
        'password' : user['login']['password'],
        'email' : user['email']
    })
    processed_user.to_csv('/tmp/processed_user.csv', index=None, header=False)

	processing_user = PythonOperator(
	        task_id="processing_user",
	        python_callable=_processing_user
	    )

'''
exracting_user에서 db에 data를 넣고 xcom를 통해 db에서 pull 해온다.
'''

----------test-----------
airflow tasks test user_processing processing_user 2020-01-01
cat /tmp/processed_user.csv
```

**전처리한 데이터 저장하기 → user_processing.py**

```python
from airflow.operators.bash import BashOperator

storing_user = BashOperator(
        task_id = 'storing_user',
        bash_command='echo -e ".separator ","\n.import /tmp/processed_user.csv users" | sqlite3 /home/airflow/airflow/airflow.db'
    )

----test-----
airflow tasks test user_processing storing_user 2020-01-01
```

**dependency 추가 및 refactoring**

```python

# 의존성 추가
creating_table >> is_api_available >> extracting_user >> processing_user >> storing_user

# creating table 작업 시 여러번 요청에 대응하기 위한 조건 추가
CREATE TABLE IF NOT EXISTS users

# 작업이 잘못됬을 시 graph view에서 노드 클릭하여 초기화 후 재실행 해주기
```

### **airflow’s scheduling**

**정의**

- airflow scheduler를 실행 시키면, 모든 Dag와 task들을 모니터링 한다.

**유의사항**

- start_date가 2020-01-01인 경우, task는 start_date가 끝난 직후인 2020-01-02에 실행하게된다.

**catch-up option**

해당 옵션을 true로 바꾸면 start date부터 현재 execution date까지의 모든 작업을 backfill 된다.

- ex) start_date=2020-01-01, execution date=2022-01-01, interval=@daily
    - 2020-01-01부터 execution까지 하루 간격으로 task를 생성한다.

endpoint가 어떻게 작용하는지
