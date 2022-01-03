### DevOps란?

개발과 운영을 함께하는 문화

### 네트워크란?

연결을 위한 시스템, IT분양

2개 이상의 node(컴퓨터)끼리 통신을 하기 위한 것

**네트워크 단위의 명칭**

패킷  프레임  세그먼트  데이터그램

### 프로토콜

서로 통신하기 위한 네트워크 규칙

계층 모델인 OSI-7 예시. 이 규칙만 잘 지키면 타 노드와 호환된다.

![https://user-images.githubusercontent.com/70089259/147895360-fa036406-58e1-4026-940b-9395704f1488.png](https://user-images.githubusercontent.com/70089259/147895360-fa036406-58e1-4026-940b-9395704f1488.png)

**모듈화**

복잡한 시스템을 기능별로 여러 개의 작고 단순한 모듈로 독립화

계층 구조와 유사하다

![https://user-images.githubusercontent.com/70089259/147895480-f56fbaeb-fbfb-419f-9a5b-a5cf3c6b8c74.png](https://user-images.githubusercontent.com/70089259/147895480-f56fbaeb-fbfb-419f-9a5b-a5cf3c6b8c74.png)

### OSI 모델

![https://user-images.githubusercontent.com/70089259/147895637-d0badafc-81b0-4e9a-8a3e-c9a9f0a5f7be.png](https://user-images.githubusercontent.com/70089259/147895637-d0badafc-81b0-4e9a-8a3e-c9a9f0a5f7be.png)

**데이터 단위**

APDU  PPDU  SPDU

TPDU : 세그먼트(TCP) 데이터그램(UDP)

NPDU : 패킷

DPDU : 프레임

**물리 계층**

물리적인 연결, 전기적 신호로 변환하여 전송매체를 통해 수신 측에 보낸다.

전송매체 : 허브 라우터 네트워크 카드 케이블 등

데이터 단위 - APDU

**데이터 링크 계층**

무/유선 등에 따른 통신 방식을 정한다,

데이터 전송을 위한 포맷결정

물리적인 주소 사용(MAC주소) 

물리적 링크를 이용하여 신뢰성 있는 데이터를 전송하는 계층, 전송로 역할

비트를 프레임이라는 논리적 단위로 구성하는데, 전송하려는 데이터에 인접하는 노드의 주소가 정해진다.

오류를 줄이기 위해 네트워크 계층에서 받은 패킷을 프레임으로 구성하여 물리 계층으로 전송한다.

트레일러가 오류를 검출하는 특별한 비트가 있다.

데이터 단위 - PPDU

**네트워크 계층**

논리적인 주소 사용

경로 관리, 최적 경로 결정

데이터를 패킷 단위로 분할하여 전송 후 재결합 한다.

데이터 단위 - NPDU(패킷)

**전송 계층**

End to End connection

논리적인 통로 제공

신뢰성 있는 전송 보장

데이터 단위 - TPDU(TCP-세그먼트, UDP-데이터그램)

**세션 계층**

응용 프로그램 계층 사이의 접속을 설정/ 유지/ 종료 시켜주는 역할

통신장치 간의 설정을 유지하고 동기화하는 역할

데이터 단위 - SPDU

**표현 계층**

데이터 포현 차이를 해결하고 공통 형식을 제공

송신 측과 수신 측 사이에서 표준화된 데이터 형식을 규정

데이터 단위 - PPDU

**응용 계층**

파일 전송, 데이터베이스, 원격 접속 등 응용 서비스를 네트워크에 접속시키는 역할을 하며, 여러가지 서비스를 제공한다.

데이터 단위 - APDU

**정리**

![https://user-images.githubusercontent.com/70089259/147912355-bd4e3f26-469b-445a-89a4-f5a4119e551d.png](https://user-images.githubusercontent.com/70089259/147912355-bd4e3f26-469b-445a-89a4-f5a4119e551d.png)

**데이터의 전송**

![https://user-images.githubusercontent.com/70089259/147896524-14b3e867-21ec-4953-a1ae-6cb8e507a750.png](https://user-images.githubusercontent.com/70089259/147896524-14b3e867-21ec-4953-a1ae-6cb8e507a750.png)

### 네트워크 접속장치

**LAN 카드** : 맥 주소

**중계기** : 근거리 통신망 확장 및 연결

**허브** : 3대 이상의 컴퓨터를 연결하는 장치

스위칭 허브 : 스위칭 테이블을 이용하여 원하는 포트에만 전송

더미 허브 : 전체 대역폭을 노드 수만 큼 나누기 때문에 소규모 네트워크 구축 시 사용

스캐커블 : 여러대의 허브를 하나의 허브처럼

**스위치 :** 브리지 기능을 포함 ****

**브리지 :** 충돌과 통신 속도 해결

**게이트웨이 :** 서로 다른 네트워크가 통신하기 위해 거치는 장비, 서로 다른 프로토콜도 변환하여 정보를 주고 받는다. 현대에서는 장비가 아닌 개념으로 사용

**라우터** : 일반적으로 게이트웨이의 역할을 한다. 라우팅 테이블을 사용하여 경로도 찾아준다. 

**1계층 장비**

더미허브 중계기(리피터) → 전기 신호 증폭 및 분배

**2계층 장비**

스위치 브릿지 스위칭허브 → 스위칭 기능, 물리주소를 사용

**3계층 장비**

라우터 L3 스위치 → IP 주소에 따른 경로 추천 및 선택(라우팅), IP주소(논리적 주소)를 사용

### 네트워크 전송 매체(유선)

**정의**

송신 수신 측 사이를 상호 연결하는 물리적 선로

![https://user-images.githubusercontent.com/70089259/147902420-46682811-3231-43be-9c40-828127728d50.png](https://user-images.githubusercontent.com/70089259/147902420-46682811-3231-43be-9c40-828127728d50.png)

**동축 케이블**

- STP UTP 보다 전송거리가 길고, 광케이블 보다 저렴, 취급이 어렵다
- 꼬임선보다 주파수가 높고 데이터 전송이 빠르다, 외부 신호와 전자파를 차단하는 능력이 뛰어나다
- 주로 유선방송 근거리 통신망 등에 사용

**꼬임선(UTP, FTP, STP)**

**UTP**

- 대표적인 랜 케이블, 설치가 쉽고 다른 매체에 비해 저렴하다
- 외부 및 전기적인 간섭에 약하다, 상대적으로 전송거리가 짧으며 느리다.
- 크로스 오버 케이블, 서로 같은 종류 연결 시
    - switch-[switch, hub]
    - hub - [hub]
    - router - [router, pc]
    - pc - [pc]
- 스트레이트 케이블, 서로 다른 종류 연결 시
    - switch- [router, pc, server]
    - hub - [pc, server]

**STP**

- 모든 외부 간섭에서 탁월하다
- 비용이 비싸며 UTP보다 설치가 까다롭다

**광케이블**

single mode : core를 좁게 하여 큰 정밀도를 갖는다. 신호의 감쇄가 적음

multi mode : 복수의 광신호가 통과할 수 있다.

![https://user-images.githubusercontent.com/70089259/147904540-8e754860-fa83-40bc-9ee3-f54eebdb8ce2.png](https://user-images.githubusercontent.com/70089259/147904540-8e754860-fa83-40bc-9ee3-f54eebdb8ce2.png)

### 네트워크 전송 매체(유선)

**라디오파**

빛의 속도로 데이터 전송, 대기를 통과하기에 데이터 전송에 유용.

FM - 직선, 멀리 가지만 곡선을 못감

AM - 곡선, 신호는 약하지만 곡선을 잘타고 감 (산쪽에서는 AM이 잘 터진다)

### 네트워크 접속형태

**종류**

성형  트리형  버스형  링형  그물형

**성형**

![https://user-images.githubusercontent.com/70089259/147906201-59946b43-05a9-4bb6-94de-a49e04876aaf.png](https://user-images.githubusercontent.com/70089259/147906201-59946b43-05a9-4bb6-94de-a49e04876aaf.png)

**버스형**

![https://user-images.githubusercontent.com/70089259/147906288-160df164-85c6-4762-8381-89834dc822ab.png](https://user-images.githubusercontent.com/70089259/147906288-160df164-85c6-4762-8381-89834dc822ab.png)

**트리형**

![https://user-images.githubusercontent.com/70089259/147906674-9d36071f-2bdf-4bb4-a48a-8d05a264f259.png](https://user-images.githubusercontent.com/70089259/147906674-9d36071f-2bdf-4bb4-a48a-8d05a264f259.png)

**링형**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3add9491-997d-4dd9-aa97-7ea635540a06/Untitled.png)

버스형에서의 단점인 동시에 데이터가 지나다닐 때 충돌이 일어나는 일을 방지한다.

**그물형**

![https://user-images.githubusercontent.com/70089259/147907192-8f708ba6-5037-4ef9-be21-b87645b6ae33.png](https://user-images.githubusercontent.com/70089259/147907192-8f708ba6-5037-4ef9-be21-b87645b6ae33.png)

중앙제어 노드가 없다

왜 하나의 노드가 추가될 때 기존에 있던 노드에 랜카드가 추가되야하나

**혼합형**

![https://user-images.githubusercontent.com/70089259/147907446-af83f091-989a-4c54-b32f-295ab5f4ef7b.png](https://user-images.githubusercontent.com/70089259/147907446-af83f091-989a-4c54-b32f-295ab5f4ef7b.png)

### 통신방식

다른 컴퓨터에 데이터 전송 서비스를 제공하는 컴퓨터를 서버, 서비스를 수신하는 컴퓨터를 클라이언트 = 서버/클라이언트 시스템

**유니캐스트(LAN)**

1대1 통신 방식, 전송되는 프레임 안에 맥주소가 있어야한다.

**브로드 캐스트(LAN)**

1대 다로 통신하는 데이터 전송 서비스

원하지 않는 클라이언트도 수신을 해야하므로 성능저하를 불러올수있다.

**멀티캐스트(LAN)**

유니캐스트 브로드캐스 단점을 보완한 방식
