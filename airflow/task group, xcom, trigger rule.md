# task group, xcom, trigger rule - section5

**TaskGroup으로** **subdags 만들기** → **parallel_dag.py** 

```python
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from airflow.operators.subdag import SubDagOperator

from subdag.subdag_parallel_dag import subdag_parallel_dag
from airflow.utils.task_group import TaskGroup

from datetime import datetime

default_args = {
    'start_date': datetime(2020, 1, 1),
}

with DAG(dag_id='parallel_dag', schedule_interval='@daily', 
    default_args=default_args, catchup = False) as dag:
    
    # Tasks dynamically generated 
    task_1 = BashOperator(
        task_id='task_1', 
        bash_command='sleep 3'
    )
    
    with TaskGroup('processing_tasks') as processing_tasks: 
        task_2 = BashOperator(
            task_id='task_2', 
            bash_command='sleep 3'
        )
				# spark_tasks.task_3를 내부적으로 가지므로 중복가능하다.
        with TaskGroup('spark_tasks') as spark_tasks:
            task_3 = BashOperator(
                task_id='task_3', 
                bash_command='sleep 3'
            )
        
        with TaskGroup('flink_tasks') as flink_tasks:
            task_3 = BashOperator(
                task_id='task_3', 
                bash_command='sleep 3'
            ) 
            
    task_4 = BashOperator(
        task_id='task_4', 
        bash_command='sleep 3'
    )

    task_1 >> processing_tasks >> task_4
```

**task 간에 data 공유 방법**

1. 외부 툴에서 push pull
2. airflow 자체 db를 이용하는 xcom

**xcom을 사용하여 push 및 pull → xcomdag.py** 

```python
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator
from airflow.operators.subdag import SubDagOperator
from airflow.utils.task_group import TaskGroup

from random import uniform
from datetime import datetime

default_args = {
    'start_date': datetime(2020, 1, 1)
}

def _training_model(ti):
    accuracy = uniform(0.1, 10.0)
    print(f'model\'s accuracy: {accuracy}')
    ti.xcom_push(key='model_accuracy', value=accuracy)

def _choose_best_model(ti):
    print('choose best model')
    accuracies = ti.xcom_pull(key='model_accuracy', task_ids = [
        'processing_tasks.training_model_a',
        'processing_tasks.training_model_b',
        'processing_tasks.training_model_c'
    ])

with DAG('xcom_dag', schedule_interval='@daily', default_args=default_args, catchup=False) as dag:

    downloading_data = BashOperator(
        task_id='downloading_data',
        bash_command='sleep 3'
			#	do_xcom_push = False
    )

 
```

- 기본적으로 몇 Operator는 자동적으로 xcom이 생성되고 옵션으로 끌 수 있다.
    - do_xcom_push = False

**BranchPythonOperator & DummyOperator를 이용한 조건 Dag → xcomdag.py**

```python
from airflow.operators.python import BranchPythonOperator
from airflow.operators.dummy import DummyOperator

def _choose_best_model(ti):
    print('choose best model')
    accuracies = ti.xcom_pull(key='model_accuracy', task_ids = [
        'processing_tasks.training_model_a',
        'processing_tasks.training_model_b',
        'processing_tasks.training_model_c'
    ])

    for accuracy in accuracies:
        if accuracy > 5:
            return 'accurate' 
    return 'inaccurate'  # ['accurate', 'inaccurate'] 로 multi tasks 가능

# accurate inaccurate는 조건에 의해 스킵되거나 실행된다.
downloading_data >> processing_tasks >> choose_model
    choose_model >>  [accurate, inaccurate]

```

**trigger rule을 이용한 storing → xcomdag.py**

```python
storing = DummyOperator(
        task_id='storing'
				trigger_rule='none_failed_or_skipped'
    )

    downloading_data >> processing_tasks >> choose_model
    choose_model >>  [accurate, inaccurate] >> storing

# storing에 트리거룰이 없다면, 
# [accurate, inaccurate] 이 중 하나는 스킵되면서 storing도 스킵이된다.
```

**트리거룰** 

- 설명은 [a, b] >> c
- 코드는 [1,2] >>3, 전부 3번이 성공하도록 짜여있다.
- exit 0 → success , exit 1 → failed
- **all_success**
    
    ```python
    with DAG(dag_id='trigger_rule', schedule_interval='@daily', 
        default_args=default_args, catchup = False) as dag:
    
            task_1 = BashOperator(
                task_id='task_1'
                bash_command='exit 0'
                do_xcom_push=False
            )
    
            task_2 = BashOperator(
                task_id='task_2'
                bash_command='exit 0'
                do_xcom_push=False
            )
    
            task_3 = BashOperator(
                task_id='task_3'
                bash_command='exit 0'
                do_xcom_push=False
            )
    ```
    
    - a, b 성공  c 진행
    - a, b 중 하나라도 실패하면 c는 upstream_failed status
- **all_failed**
    
    ```python
    task_1 = BashOperator(
                task_id='task_1'
                bash_command='exit 1'
            )
    
            task_2 = BashOperator(
                task_id='task_2'
                bash_command='exit 1'
            )
    
            task_3 = BashOperator(
                task_id='task_3'
                bash_command='exit 0'
    						trigger_rule='all_failed'
            )
    ```
    
    - a,b 실패 c 진행
    - a,b 둘 중 하나라도 성공하면 c는 skip
- **all_done**
    
    ```python
    task_1 = BashOperator(
                task_id='task_1'
                bash_command='exit 1'
            )
    
            task_2 = BashOperator(
                task_id='task_2'
                bash_command='exit 0'
            )
    
            task_3 = BashOperator(
                task_id='task_3'
                bash_command='exit 0'
    						trigger_rule='all_done'
            )
    ```
    
    - a,b 상태와 상관없이 trigger되면 c는 실행된다.
- **one_success**
    - a, b 둘 중 하나라도 성공하면 c는 실행
- **one_failed**
    
    ```python
    task_1 = BashOperator(
                task_id='task_1'
                bash_command='exit 1'
            )
    
            task_2 = BashOperator(
                task_id='task_2'
                bash_command='sleep30'
            )
    
            task_3 = BashOperator(
                task_id='task_3'
                bash_command='exit 0'
    						trigger_rule='one_failed'
            )
    
    # 2가 동작 중이더라도 1이 실패했기에 3번은 동작한다.
    ```
    
    - a, b 둘 중 하나라도 실패하면 c는 실행
- **none_failed**
    - a,b 둘 중 failed 상태만 아니라면 성공하거나 스킵하면 c 트리거
- **none_failed or skipped**
    - none_failed는 a,b 상태가 통일되야되지만 해당 status는 상관없다. 하지만 하나라도 성공해야한다. ex) a성공 b스킵 되었을 때 filed가 없고 하나 이상이 통과되었기에 c는 트리거될 조건 만족한다.
