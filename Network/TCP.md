### 프로토콜 기초

정보프레임 I :  상위 계층이 전송 요구한 데이터를 송신하는 용도, 순서번호, 송수신 호스트 정보 등

긍정프레임 ACK : 전송한 데이터가 올바르게 송신되었는지

부정프레임 NAK : 전송 데이터가 오류가 있어서 재전송 요청하는 용도

**피기배킹**

정보프레임 긍정프레임 따로 따로 패킷을 보내면 복잡하므로 둘이 합쳐서 보내는 개념

표기법 : I(m, n)

### TCP

**정의**

연결지향으로 신뢰성과 순서적인 데이터 전달 서비스

**포트번호**

TCP가 상위 계층과 데이터 송수신을 할 때 사용하는 데이터의 이동 통로이다. 

ftp telnet 등 상위 계층들과 서로 다른 포트를 사용하기에 동시 다운로드 가능하다.(0~65534)

0~1023 → well-know port  ex) 웹 서버에 접속 시 사용되는 http프로토콜은 자동으로 80번 사용

1024~49151 → 등록된 포트

나머지 → 동적 `포트

**TCP 세그먼트**

바이트 스트림을 세그먼트 단위로 나눔

**TCP 헤더 구조**

![https://user-images.githubusercontent.com/70089259/148481385-3e49bcbd-f8fc-49c0-870e-89dd74e31570.png](https://user-images.githubusercontent.com/70089259/148481385-3e49bcbd-f8fc-49c0-870e-89dd74e31570.png)

- Sequence **Number(순서번호)**
    - ****
- **Acknowledgement Number(응답번호)**
    - 수신하기를 기대하는 다음 바이트 번호 = 마지막 수신 성공 번호 + 1

- **HLEN(헤더 길이 필드)**
    - 32비트 단위, 5~15까지의 값을 가진다. 최소 5*32

- **예약 필드(3bit임) 새로 3개 추가됌**

- **Flag bits(NS CWR ECE + URG ACK PSH RST SYN FIN)**
    - 프로토콜의 동작을 제어하는데 사용하는 비트 단위의 플래그
    - NS
    - CWR
    - ECE
    - URG
    - ACK
    - PSH
    - RST
    - SYN
    - FIN
- 윈도우크기
- 검사합
- 긴급포인터
- 옵션

통신하면서 일어나는 일, syn을 보내면 syn_sent,received상태에서 후에 두 측 모두 establish 상태가 된다. → 3way-handshake

![https://user-images.githubusercontent.com/70089259/148486324-19b1185c-dd5f-40d4-be6b-03ccd38b6cab.png](https://user-images.githubusercontent.com/70089259/148486324-19b1185c-dd5f-40d4-be6b-03ccd38b6cab.png)

hi →

←hello

bye →

← bye 하는 예제 

통신을 연결해놓기 위해 ack 3001 재전송, s측에 다음에 3001번으로 보내라는 소리

![https://user-images.githubusercontent.com/70089259/148487232-18b2d2af-3a09-47f4-8d44-b94bea3db6a5.png](https://user-images.githubusercontent.com/70089259/148487232-18b2d2af-3a09-47f4-8d44-b94bea3db6a5.png)

1001번 받고 hi의 바이트 수 만큼 올려준 ack와 함께 전송, c측에서 받고 hello만큼 키워서 전송한다

![https://user-images.githubusercontent.com/70089259/148487306-8df1e82c-a212-4e63-881f-256da03f5074.png](https://user-images.githubusercontent.com/70089259/148487306-8df1e82c-a212-4e63-881f-256da03f5074.png)

![https://user-images.githubusercontent.com/70089259/148487390-74443fb9-0dc1-4dc3-84ce-3c7195d8f754.png](https://user-images.githubusercontent.com/70089259/148487390-74443fb9-0dc1-4dc3-84ce-3c7195d8f754.png)

종료를 위한 4way handshake실행

받고 종료할 때 Fin을 보내주면 s측은 closed상태로 들어간다. 

c측은 fin을 보내고 혹시 모르니 wait상태로 들어가고,

s측이 마지막으로 last_ack로 fin을보낸다

c측은 time_wait상태 후 닫히고 s측은 ack를 받은 즉시 closed상태가 된다.

![https://user-images.githubusercontent.com/70089259/148487704-ca607ffd-5566-4484-b27c-d14feb27c5ca.png](https://user-images.githubusercontent.com/70089259/148487704-ca607ffd-5566-4484-b27c-d14feb27c5ca.png)

**슬라이딩 윈도우**

송신 버퍼 역할

**UDP**

비연결지향으로 간단함

 **ARP(Address Resolution Protocol)**

IP주소를 물리적 주소로 대응하기 위한 프로토콜

3계층 프로토콜이지만, 라우터까지 거치지 않는다. 단순히 바꾸기 위해서만 IP를 사용

3계층의 밑바닥

**ICMP(Internet Control Message Protocol)**

TCP/IP에서 IP패킷을 처리할 때 발생되는 문제를 알리거나 기타 기능 등을 수행하는 프로토콜

IP의 상위 계층 처럼 행동하나 IP에 속해있다.

다양한 정보를 얻기위한 프로토콜
