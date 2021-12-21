## Background

**Virtual Memory**

- 프로그램이 피지컬 메모리보다 클 경우에도 프로세스가 실행되도록 하는 테크닉

![https://user-images.githubusercontent.com/70089259/146786878-1cd99115-1dcd-4056-90fc-daceb76d8597.png](https://user-images.githubusercontent.com/70089259/146786878-1cd99115-1dcd-4056-90fc-daceb76d8597.png)

**Virtual Address Space**

![https://user-images.githubusercontent.com/70089259/146787236-e1b3c60a-f6a9-4ccf-a51a-480fa61c3006.png](https://user-images.githubusercontent.com/70089259/146787236-e1b3c60a-f6a9-4ccf-a51a-480fa61c3006.png)

**Virtual Memory 장점**

- 여러 프로세스가 page를 공유 할 수 있다.

![https://user-images.githubusercontent.com/70089259/146788613-029d8fe2-e014-48aa-8774-0434924219f8.png](https://user-images.githubusercontent.com/70089259/146788613-029d8fe2-e014-48aa-8774-0434924219f8.png)

## Demand Paging

**프로그램(하드웨어)가 실행되어 프로세스(메모리)로 바뀌는 내부동작**

1. 저장소에서 메모리로 올라간다
2. 모든내용을 피지컬메모리에 올리지 않는다.(아두이노 등 제외)
3. page로 분할 시키고 요청 될 때만 부분만 올린다(=demand paging)

**Page Fault**

1. 프로세스의 일부(page)가 시작을 하기전 page table을 확인한다
2. 프로세스의 원하는 페이지가 메모리에 있는지 확인한다
3. 만약 invalid하다면, page를 메모리에 넣어줘야한다
4. free-frame을 찾아서 second storage에서 넘겨준다
5. page table을 reset을 통해 valid로 표시해준다
6. 다시 인스트럭션을 그대로 실행해준다

![https://user-images.githubusercontent.com/70089259/146925907-47782d2c-0958-41c0-a00b-8d9148d2f427.png](https://user-images.githubusercontent.com/70089259/146925907-47782d2c-0958-41c0-a00b-8d9148d2f427.png)

**Pure Demand Paging**

요청을 하지 않으면 페이지를 가져오지 않기에, 첫번째 지시부터 바로 page fault를 일으킨다.

- 단점
    
    3개의 page를 연속해서 실행할 때 3연속의 page fault를 일으키므로 비효율적이다
    
- Locality of Reference
    
    코드 혹은 데이터의 집약성 때문에 한번 호출된 페이지에서 반복해서 작업이 일어나지, 계속 다른 페이지를 찾는 경우는 적다
    

**Hardware Support to Demand Paging**

- Page table - valid or invalid mark
- Secondary storages - swap space

**Instruction Restart**

paging out, in을 하는 중, page table이 관리가 안되면 꼬일 수 있다.

**Demanding paging에 영향을 주는 요소**

- TLB에 있을 확률
- page fault
    
    trap걸어주는 시간
    
    page를 읽는 시간
    
    프로세스를 다시 시작하는 시간
    

## Copy-on-Write

fork() or exec()처럼 같은 프로세스를 복사하는 경우는 첫번째 사진 처럼 같은 메모리를 공유하다가 write를 통해 작업공간이 달라지는 경우만 copy를 하는 개념이다

![https://user-images.githubusercontent.com/70089259/146932822-fce105de-0718-47ae-8023-b42fbc7f930c.png](https://user-images.githubusercontent.com/70089259/146932822-fce105de-0718-47ae-8023-b42fbc7f930c.png)
