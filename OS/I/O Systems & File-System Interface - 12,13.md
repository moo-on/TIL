## I/O System

**컴퓨터가 가장 많이 하는 작업은?**

I/O 작업이 압도적으로 작업량이 많다.

**PCIe bus**

해당 버스형 구조에 각종 컨트롤러가 달려있고 이를 디바이스가 처리할 수 있도록 cpu가 전부 관리해준다.

**Memory-Mapped I/O**

I/O address에 device연결할 수있도록 메모리를 주소를 맵핑 시켜둬서 제어를 한다.

**three types of I/O**

- polling : busy-waiting(계속 확인)
- interrupt : interrupt service routine에 제어권을 넘겨줘서 활용, interrupt vector table을 이용한다.
- DMA : register를 통해 load store move 등 작업을 기다리기 싫을 때 하드웨어 버스를 통해 바로 접근한다

**sync vs async**

호출되는 함수의 작업 완료 여부를 누가 신경쓰는지에 따른다.

- sync :  호출된 함수의 작업 완료 후 리턴을 기다리거나, 리턴이 되더라도 작업 완료 여부를 호출하는 함수가 계속 신경쓰는 것
- async : 호출된 함수에게 callback을 전달, 호출된 함수는 작업이 완료되면 callback을 다시 호출한다. 그 동안 호출하는 함수는 작업 완료 여부를 신경쓰지 않는다.

**Blocking I/O**

호출된 함수가 자신의 작업을 마칠 때까지 호출한 함수에게 제어권을 안주고 대기시키는 것

- **sync :** 함수가 호출된다면, 기존 작업은 멈추고 호출된 함수가 완료 후에 되돌아와서 실행한다
- **async :** sync와 큰차이가 없다. 다만 기존 작업의 호출과 callback이 같이 되고, 호출된 함수가 callback을 주면 기존 작업으로 돌아가진다.
    
    

**Non-Blocking I/O**

호출된 함수가 바로 리턴을 하여 호출한 함수에게 제어권을 넘겨줘서 일을 하게 해주는 것

- sync : 새로운 함수 호출 직후 바로 리턴을 받았지만, 작업 완료 여부를 지속적으로 묻는다.
- async : 새로운 함수 호출 직후(callback을 넘겨줌) 바로 리턴을 받고 작업을 한다. 그 후 호출했던 함수가 다시 callback을 호출한다.

## File-System Interface, Implementation

File system에서는 file과 directory가 있다.

**Access Method**

- sequential-access : 과거 마그네틱 테이프
- direct-access : 랜덤 엑세스

**file system itself**

![https://user-images.githubusercontent.com/70089259/148392308-d7973df4-a934-4b65-b166-7dd72a08396d.png](https://user-images.githubusercontent.com/70089259/148392308-d7973df4-a934-4b65-b166-7dd72a08396d.png)

**Allocation Method**

devices(하드디스크)에 어떻게 효율적으로 파일을 읽고 쓰고할지.

- Contiguous Allocation : external fragmentation발생
- Linked allocation : 파일 조각들이 링크드 되있어서 조합을 한다. 동영상 파일 되돌아가기 등에 취약
- File Allocation Table : 어떤 볼륨에 linked-list index를 집어넣어 좀 더 효율적으로 사용하자
- indexed allocation : bad sector 문제점 해결

![https://user-images.githubusercontent.com/70089259/148394754-882c415d-d27a-46e5-84c9-9909d645640e.png](https://user-images.githubusercontent.com/70089259/148394754-882c415d-d27a-46e5-84c9-9909d645640e.png)

linux에서 사용하는 파일 포맷 → ELF

**Free-space Management**

free-space list head를 알고있어야 서빙할 수 있음
