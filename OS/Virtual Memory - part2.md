## Page Replacement

**Page Replacement 알고리즘이 필요한 이유**

frame의 개수가 40개, 현재 동작하고 있는 프로세스가 가진 페이지가 60개더라도 Demand-Paging의 개념으로 모든 페이지를 올리지 않고 일부 페이지만 physical memory에 올림으로 여유 프레임을 확보할 수 있었다. 그렇지만 페이지수가 더 많은 프로세스 혹은 요구될 경우 기존에 있는 페이지를 교체해야하는 상황이 있기에 페이지 교체 알고리즘은 필요하다.

**페이지가 교체되는 과정**

secondary-storage에서 frame을 요구 → 기존에 있는 페이지 중에 희생자를 찾기 → 희생자를 빼고 해당 page table에 invalid 설정 → 요구된 page를 memory에 올리기 → 해당 page table에 valid 설정

**페이지 요구와 교체가 중요한 이유**

secondary storage I/O의 동작은 자원이 많이 들어가기에 조금의 환경 개선으로 많은 퍼포먼스를 얻을수 있다.

Page Fault를 카운트해서, 이 개수를 최소화 하는것이 목표

**어떤 알고리즘을 써야할까?**

간단하게FIFO알고리즘을 쓰면 효율적이지 않고, 크기가 커져도 일정수의 페이지 폴트가 반복되는 벨레이디의 모순 이라는 문제점이 있다.

OPT or MIN 이라고 불리는 미래 사건에 대해 알수 있다면, 가장 쓰이지 않는 페이지를 빼는것이 가장 효과적인데 이는 불가능하다. 

**OPT를 근사하게 구현**

미래는 과거를 보면 알 수있다. 과거를 통해 미래를 예측해보자.

오랫동안 사용하지않은것은 미래에도 사용하지 않을것이다 → LRU(Least Recently Used)

**LRU**

locality에 근거해서 가장 오랫동안 사용하지 않은 프로세스는 앞으로도 안쓰이기에, 희생자로 지정하자 → OPT처럼 밸러디의 모순을 겪지 않을 수 있다.

자료구조가 복잡하기에, 하드웨어의 지원을 통해 (counter, stack)이 구현되면 좋은데 이것도 쉽지않다.

- Counter :  실행 횟수를 세서 안 쓰는거 고르기
- Stack : 가장 밑에 깔린놈 희생자로 고르기
- reference bit
    - counter, stack이 힘드니까 쓰인거는 비트로 간단히 표현하여 최소한 안쓰인것을 고르지는 말자는 방법론
- second-chance : FIFO + reference bit

## Allocation of Frames

**Equal vs Proportional**

프로세스 크기 별로 frame 더 많이 할당할 것인가에 대한 전략

**Global vs Local**

A라는 프로세스에서 page fault가 일어난다면 A프로세스가 올라가있는 프레임을 뺄지, 전체적인 기준에서 LRU 우선순위가 낮은 프로세스에서 뺄지에 대한 전략

## Thrashing

**Thrashing이 발생하는 이유**

physical memory영역이 실질적으로 부족하게되면 잦은 Page Fault가 발생하게 된다.

invalid → valid로 요청하면서 wait큐로 들어가고 실행하게되는데 그 사이에 다시 invalid로 바뀌면서 실질적인 프로세스가 동작하지 않고 page fault만 반복하면서 CPU활용성이 심각하게 떨어지는 상태이다.

**Working Set Model**

Locality에 근거해 Page들의 모음인 working-set을 만든다.

working-set안에 위치한 프로세스가 동작 중이라면, working-set안에 있는 프로세스에는 fault를 내지 말자는 모델
