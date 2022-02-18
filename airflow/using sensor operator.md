# using sensor operator - section 3

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f7256226-5fbb-4759-a609-7de221c7c4af/Untitled.png)

**Dag 시작하기**

```python
from airflow import DAG

from datetime import datetime, timedelta

default_args = {
    "owner": "airflow",
    "email_on_failure": False,
    "email_on_retry": False,
    "email": "admin@localhost.com", # if failure or retry, notify to email
    "retries": 1, # limits retry
    "retry_delay": timedelta(minutes=5)
}

# catch up 옵션이 true일 경우, start_date ~ 현재까지 실행 안됬던 모든 대그가 실행된다.
with DAG("forex_data_pipeline", start_date=datetime(2021, 1 ,1), 
    schedule_interval="@daily", default_args=default_args, catchup=False) as dag:
    None
```

**Operator**

Operator는 task이다.

**Operator의 3가지 유형**

Action

- 로직을 가지고 실행된다.

Transfer

- 단순하게 데이터의 이동 등 흐름을 만든다.

Sensor

- 신뢰성있는 작업을 위해서, 다음 작업을 하기 위해 대기하는 동작을 한다.

**provider 모듈**

airflow.providers.{user_setting}

[https://airflow.apache.org/docs/apache-airflow-providers/packages-ref.html](https://airflow.apache.org/docs/apache-airflow-providers/packages-ref.html)

**데이터파이프라인예시**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/93f02dd1-3966-4b28-a5e0-05cb2ba027d9/Untitled.png)

1. **using http sensor → check availability api**

```python
# forex api를 이용할 것이기에, http센서(~.http.sensors.http)를 사용 
# → 60초마다 확인이 가능하다.

is_forex_rates_available = HttpSensor(
	task_id="is_forex_rates_available",
	http_conn_id="forex_api",  # create custom api
	endpoint="marclamberti/f45f872dea4dfd3eaa015a4a1af4b39b", 
	response_check=lambda response: "rates" in response.text,
	poke_interval=5,  # 5초마다 api가 이용가능한지 체크한다.
  timeout=20  # 응답이 없으면 20초 후에 자동으로 타임아웃한다. 
)

# http_conn_id="forex_api"
# 이 부분은 따로 connection을 생성해준다.
```

1. using file sensor → check availability file having
