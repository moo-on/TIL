## Paging

**Paging의 정의**

- CMA의 contiguous한 구조를 non-contiguous하게 바꾼다.
    - external fragmentation을 피한다
    - 하드웨어의 도움을 받아서 작동한다
    

**Paging의 Method**

- physical memory를 고정된 크기로 나눈다 - frame
- logical memory는 같은 사이즈로 해당 블럭으로 들어간다 - page
    - logical memory를 1,2로 나눠줬다면 둘이 연속된 공간으로 할당될 필요가 없다
- logical address space와 physical address space는 완벽하게 분리가 된다.
    - logical주소와 연결된 물리적인 주소는 운영체제가 알아서 해준다

**Page number**

- 모든 주소는 CPU의해 2개의 파츠로 나누어진다
    1. (p, d) = (page number, page offset)
    2. page table이 존재하고 p라는 페이지를 찾아가면, physical address를 알게된다
    3. 알게된 frame이라는 physical memory space로 들어가서 d라는 offset만큼 읽어준다
    
    ![https://user-images.githubusercontent.com/70089259/146400883-2e3153b5-4539-46a1-8b03-94b67d0a3489.png](https://user-images.githubusercontent.com/70089259/146400883-2e3153b5-4539-46a1-8b03-94b67d0a3489.png)
    

**Page size**

- one page size = 2**2
- all page size = 2**10
- page number를 표현할 때는 10bit필요하다
- page offset을 표현할 때는 2bit필요하다

**Hardware Support**

- 페이지 사이즈가 너무 크게되면 하드웨어가 관리하기 힘들다
- PTBR(page-table base register)
    - page의 시작번지를 가르키자
    - page table은 메인 메모리에 둔다
    - PTBR만 PCB에저장하기에 Context switch가 빠르다(전체 페이지를 저장 안함)
    - 대신, 메모리 엑세스는 느리다
    - 메모리를 2번 엑세스해야한다
        
        

**TLB(Translation Look-aside Buffer)**

- page table이 크기에, 캐시메모리를 사용하여 physical memory에 빠르게 엑세스한다
- TLB hit - TLB안에 page number가 포함되서 빠르게 엑세스 하는 경우
- TLB miss - TLB안에 page number가 없어서 메인 메모리에 있는 page number를 가지고 오는 경우
- hit ratio - hit와 miss의 비율

**Memory Protection with Paging**

- 기존 contiguous는 base와 limit로 쉽게 valid를 판단했다
- valid-invalid bit: 하드웨어적으로 one bit를 추가해서 해당 문제를 체크한다

**Shared Pages**

- paging을 통해 해당 기능을 쓰면 공통의 코드를 공유하기 좋다, 멀티프로그래밍의 환경에서
- Reentrant code
    - 실행 중 바뀔 일이 없고 전부 공유해서 쓸 수 있는코드(print함수)
    

**Page Table의 구조**

- Hierarchical Paging
- Hashed Page Table
- Inverted Page Table -

**Swapping**

- real physical memory보다 logical memory가 더 클 경우, 현재 쓰는 페이지만 physical에 두자.
- 프로세스 당 당장 쓸 페이지만 올리니까 동시에 더 많은 것을 돌릴 수 있다.
- 엑세스할 경우만 메모리에 두는 기법
- Page 단위로 이루어지며, 오늘 날의 Paging 기법은 Swapping을 일컫는다.

![https://user-images.githubusercontent.com/70089259/146557471-b92dcfbe-6f99-443a-a4e3-6524e7ffdd9a.png](https://user-images.githubusercontent.com/70089259/146557471-b92dcfbe-6f99-443a-a4e3-6524e7ffdd9a.png)
