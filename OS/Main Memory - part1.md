## Background

프로세스란 실행 중인 프로그램이며, main memory에 올라간 상태이다.

**메모리 구성**

- 바이트(비트 8개) 단위로 Array가 쭉 나열되있는 상태이며, 각 주소가 할당되어있다.
- CPU는 memory에서 instructions을 fecth해온다 → program counter에서 가져옴
- 가져온 메모리 역시 load 또는 store한다

**메모리 구성**

- 프로세스는 메모리 공간을 분할해서 사용한다
- base register && limit register을 가지고 합법적인 주소를 할당한다
    - 잘못짜면 segmentation fault 발생

**Address Binding**

1. 우리는 상징적으로 메모리 번지(ex int n = 3)를 저장한다 
2. 해당 메모리를 컴파일러가  relocatable 주소라고 불리는 논리적인 주소를 만든다
3. linker가 연결을 해주고 logical 주소가 생성된다
4. 실제 프로그램에 로딩되면 physical 주소를 가진다

![https://user-images.githubusercontent.com/70089259/146329823-92c1be19-ceb1-4ebc-8930-b5fadb789bf1.png](https://user-images.githubusercontent.com/70089259/146329823-92c1be19-ceb1-4ebc-8930-b5fadb789bf1.png)

**Logical vs Physical Address Space**

- user process가 사용하고 있는 주소는 Logical에 불과하다, 이를 하드웨어 메모리에 있는 physical 주소에 맵핑을 해야된다.
- 두 개는 서로 연관이 없어야 하므로, 각 주소의 공간은 분리되어야한다

**MMU(Memory Management Unit)**

- 메모리 엑세스는 수 없이 많이 일어나기에 하드웨어 영역에서 처리해준다.
- logical 주소를 physical주소로 변환 해주는 일을 한다
- relocation register - base register 역할을 한다
    1. cpu가 0x346이라는 logical address를 엑세스 하고 싶다면, 
    2. relocation register에 14000번지가 있다면, 현재 메모리는 0x14000에 엑세스 되어있다.
    3. MMU가 14346에 메모리를 쓰게 만든다 (예시)

![https://user-images.githubusercontent.com/70089259/146332434-10828053-9396-426f-af25-3ee37386a6a7.png](https://user-images.githubusercontent.com/70089259/146332434-10828053-9396-426f-af25-3ee37386a6a7.png)

**Dynamic Loading**

- 모든 것을 로딩하지 않고, 필요할때만 루틴을 로딩하자

**Dynamic Linking and Shared Libraries**

- DLLs(Dynamic Linking Libraries)
    - 시스템 라이브러리는 항상 유저프로그램과 연결되어 있다.
- static linking
    - 시스템 라이브러리를 일반 라이브러리를 불러오듯이 로더가 로딩한다
- dynamic linking
    - 링킹도 실행 시 까지 연기를 하고, 실행 시만 엑세스에서 링킹한다
- shared library
    - DLL이 shared library라고 알려짐 (ex) .dll ,.so))
    

## Contiguous Memory Allocation

**메모리를 어떻게 할당할 것인가?**

- 메모리는 os 혹은 user process가 사용한다
- 다양한 유저 프로세스가 메모리에 동시 상주 한다.
- continuous memory allocation은 프로세스를 통째로 올리기에 구조가 연속적이다(=MMU가 간단해진다)

**memory Protection**

- relocation, limit register를 주면 CMA를 썼기에 쉽다.

**Memory Allocation**

- 메모리 크기가 다르기에 어떻게 할당할지에 대한 문제
- 큰 공간의 프로세스가 빠지고, 그곳을 작은 프로세스가 어떻게 채울지
- 이러한 hole을 어떻게 다룰것인가에 대한 문제

**Dynamic Storage Allocation**

- 구멍을 어떻게 채울까?
    - First-Fit : 앞 주소 빈 공간부터 채우기
    - Best-Fit : 가장 작은 구멍부터 차근차근 채우기
    - Worst-Fit : 가장 큰 구멍부터 채우기
    

**Fragmentation(단편화문제)**

- external fragmentation - 공간이 남지만, 연속적인 공간이 없어서 나타나는 문제
- internal fragmentation - 프로세스 공간을 똑같이 나누었을 경우, 작은 크기를 넣을 경우 공간이 남는다.

**segmentation**

프로세스 역할마다 메모리 영역을 분리해놓자
