**airflow의 장점**

동적 프로그래밍, 확장 가능, ui 지원 등.. 중에

airflow의 가장 큰 장점은 확장 가능성이다.

**airflow component**

- core component
    - web server
    - scheduler
    - meta store
    - executor - 어떤 시스템에서 작동할지를 정한다.
    - worker - executing your task

**operator**

- action operator
- transfer operator
- sensor operator

**One Node Architecture**

- component
    - web server
    - scheduler
    - meta store
    - executor
- 동작방식
    - meta store의 data를 web server & scheduler가 fetch
    - scheduler는 fetch 한 data를 바탕으로 executor를 실행
    - executor는 meta store에 write.
        - queue가 있기에 순차대로 진행 가능하다.
    - 다시 web server & scheduler가 read를 반복

**multi Node Architectrue**

2분 28초 다시보기

**airflow CLI**

```python
airflow dags backfill -s 2021-01-01 -e 2021-01-05 —rest-dagruns

airflow dags trigger example_python_operator -e 2021-01-01

- Trigger the dag example_python_operator with a date in the past as execution date (This won’t trigger the tasks of that dag unless you set the option catchup=True in the DAG definition)

airflow dags trigger example_python_operator -e '2021-01-01 19:04:00+00:00'

- Trigger the dag example_python_operator with a date in the future (change the date here with one having +2 minutes later than the current date displayed in the Airflow UI). The dag will be scheduled at that date.

airflow dags list-runs -d example_python_operator

- Display the history of example_python_operator’s dag runs

airflow tasks list example_python_operator

- List the tasks contained into the example_python_operator dag

airflow tasks test example_python_operator print_the_context 2021-01-01

- Allow to test a task (print_the_context) from a given dag (example_python_operator here) without taking care of dependencies and past runs. Useful for debugging.
```
