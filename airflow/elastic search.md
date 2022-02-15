# ElasticSearch&Docker-section6

**Elastic Search**

**Docker 작동방식**

- docker file → docker image → docker run → docker container(application)
    - Docker image는 Docker hub에서 가져온다.
    - Docker run을 통해 격리된 환경에서 실행한다.
    - 격리된 환경은 같은 운영체제를 사용하기에 가상머신에 비해 컴팩트하다.

**airflow와 Docker**

- airflow의 컴포넌트의 분리
    - 웹서버 스케줄러 데이터베이스
    - 각 컴포넌트를 분리하여 사용하면 하나의 구성요소가 에러나도 다른 컴포넌트에 영향을 안주기에 효율적이다.
- Docker compose
    - 각 컴포넌트를 실행시키지 않고, compose 명령어를 통해 한번에 할수있다.
    - 이 때 각 컨테이터는 같은 네트워크를 가지고 서로 통신할 수 있다.

 

**local 환경에서의 Docker사용**

```bash

wget {airflow 설치할 수 있는 yaml파일이 있는 url}/docker-compose.yaml

docker-compose -f docker-compose.yaml up -d
```

```yaml
---
version: '3'  # docker compose 버전
x-airflow-common:  # 공통 환경 정의, 해당 코드는 여러 서비스에서 공유되기에 서비스 섹션에 없다.
  &airflow-common
  image: ${AIRFLOW_IMAGE_NAME:-apache/airflow:2.1.0}
  environment:
    &airflow-common-env  # 공통 환경 변수 설정
    AIRFLOW__CORE__EXECUTOR: CeleryExecutor
    AIRFLOW__CORE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
    AIRFLOW__CELERY__RESULT_BACKEND: db+postgresql://airflow:airflow@postgres/airflow
    AIRFLOW__CELERY__BROKER_URL: redis://:@redis:6379/0
    AIRFLOW__CORE__FERNET_KEY: ''
    AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION: 'true'
    AIRFLOW__CORE__LOAD_EXAMPLES: 'true'
    AIRFLOW__API__AUTH_BACKEND: 'airflow.api.auth.backend.basic_auth'
  volumes:  # 로컬의 ./dags 폴더가 : 이하 폴더와 동기화 된다.
    - ./dags:/opt/airflow/dags
    - ./logs:/opt/airflow/logs
    - ./plugins:/opt/airflow/plugins
  user: "${AIRFLOW_UID:-50000}:${AIRFLOW_GID:-50000}"  # 안중요
  depends_on:  # 종속성 설정 이하 어플리케이션이 실행 후 진행된다
    redis:
      condition: service_healthy
    postgres:
      condition: service_healthy
# 각 각의 어플리케이션
services:
  postgres:
    image: postgres:13
    environment:
      POSTGRES_USER: airflow
      POSTGRES_PASSWORD: airflow
      POSTGRES_DB: airflow
    volumes:
      - postgres-db-volume:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "airflow"]
      interval: 5s
      retries: 5
    restart: always

  redis:
    image: redis:latest
    ports:
      - 6379:6379
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 30s
      retries: 50
    restart: always

  airflow-webserver:
    <<: *airflow-common # 위에서 정의한 코드 불러오기
    command: webserver
    ports:
      - 8080:8080
    healthcheck:
      test: ["CMD", "curl", "--fail", "http://localhost:8080/health"]
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always

  airflow-scheduler:
    <<: *airflow-common
    command: scheduler
    healthcheck:
      test: ["CMD-SHELL", 'airflow jobs check --job-type SchedulerJob --hostname "$${HOSTNAME}"']
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always

  airflow-worker:
    <<: *airflow-common
    command: celery worker
    healthcheck:
      test:
        - "CMD-SHELL"
        - 'celery --app airflow.executors.celery_executor.app inspect ping -d "celery@$${HOSTNAME}"'
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always

  airflow-init:
    <<: *airflow-common
    command: version
    environment:
      <<: *airflow-common-env
      _AIRFLOW_DB_UPGRADE: 'true'
      _AIRFLOW_WWW_USER_CREATE: 'true'
      _AIRFLOW_WWW_USER_USERNAME: ${_AIRFLOW_WWW_USER_USERNAME:-airflow}
      _AIRFLOW_WWW_USER_PASSWORD: ${_AIRFLOW_WWW_USER_PASSWORD:-airflow}

  flower:
    <<: *airflow-common
    command: celery flower
    ports:
      - 5555:5555
    healthcheck:
      test: ["CMD", "curl", "--fail", "http://localhost:5555/"]
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always

volumes:
  postgres-db-volume:
```

**local 환경에서의 Docker사용-local_executor 사용 시**
