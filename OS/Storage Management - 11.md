## Mass-Storage

**정의**

non-volatile(비휘발성)으로 secondary storage system. 

HDD, NVM, magnetic tapes, 광역디스크(optical disk) , cloud storage → RAID system 적용

**HDD 원리**

spindle로 돌아가면서 읽는데 시간 당 얼만큼 많이 도는지로 성능 측정 가능. 

![https://user-images.githubusercontent.com/70089259/148244562-4b52dadd-42e6-4826-aae8-b3ebe168bc64.png](https://user-images.githubusercontent.com/70089259/148244562-4b52dadd-42e6-4826-aae8-b3ebe168bc64.png)

**HDD 스케줄링** 

seek time을 최소화, bandwidth를 최대화 하자

- **seek time :** 실린더가 돌아가면서 특정 섹터를 찾아가는데 걸리는 시간
- **disk bandwidth :** sector가 클수록 한번에 전송할 수 있는 용량이 크다. 시간당 전송 용량

**HDD 스케줄링 방법**

- FIFO Scheduling
- SCAN Scheduling - 왔다갔다하면서 읽기
- C-SCAN Scheduling - 한 방향으로만 읽기
    
    

**Boot Block**

bootstrap loader는 NVM flash memory에 상주하다가 전기 신호가 들어오면 켜진다.

그리고 해당 메모리는 ROM에 저장해둔다.

**Raid-System : 여러개의 디스크를 하나처럼 사용하는 방법**

parallel  - 데이터를 읽고 쓸때 병렬로 처리하는 방법

- bit-level striping : 4비트가 있다면 4개의 디스크에 동시에 쓰기
- block-level striping : 일반적으로 쪼개서 동시에 쓰기

reliability - 중복되거나깨졌을때 데이터를 보존 할 수 있는 방법

- Redundancy를 통해 충분히 미러링을 해놓는다(다른곳).

failure - 하드디스크에 손상에서도 안전할 수있는 방법

**RAID Levels 정의**

문제가 발생했는지 복구가 가능한지에 trade-off에 따라 레벨을 나누자

RAID-level을 따질 때 들어가는 개념.

- mirroring : 신뢰성이 높지만 비용이 비싸다
- striping : 효율적이지만 신뢰성하고는 관련이 없다
- parity bit : 짝수개 홀수개를 따져서 보낸다.
    - check sum → CRC로 발전

**RAID Level 구성**

- RAID 0 : 아무런 장치x, 분산만
- RAID 1 : 미러링만적용, cost 비쌈 → 굉장히 중요한 데이터만
- RAID 4 : parity bit 디스크만 따로 생성
- RAID 5 : 각 저장소에 parity bit 공간 생성 → 234의 장점을 가지고 있다. 복구에 효율성
- RAID 6 : parity bit + Q redundancy적용
- Multidimensional - RAID 6 : array로 만들어서 다차원으로 관리

![https://user-images.githubusercontent.com/70089259/148362814-683560bf-ea16-4526-97d8-70d48e593d8f.png](https://user-images.githubusercontent.com/70089259/148362814-683560bf-ea16-4526-97d8-70d48e593d8f.png)

중요한 저장소에서는 stripe를 적용해서 이러한 시스템으로 이루어져야한다.
