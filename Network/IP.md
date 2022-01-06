### IP

- 인터넷에 연결되어 있는 모든 컴퓨터가 가지고있는 고유의 주소
- 8비트 크기의 4개의 필드를 이용함 ex) [xxx.xxx.xxx.xxx](http://xxx.xxx.xxx.xxx) → IP version4
- 8비트(1바이트)는 0~255 까지의 수를 가진다. 약 43억개의 조합이 나옴

네트워크가 분리되면, 라우터가 있어야 통신할 수 있다.

### IP주소 체계

**classful address**

- 네트워크 주소와 호스트 주소로 구분
- 네트워크 ID : 라우터가 없이 통신 가능한 네트워크, 각 호스트가 속한 네트워크를 대표
- 호스트 ID : 실제 사용한 주소, 네트워크 주소로 표현되는 네트워크 내부에서 각 호스트의 주소를 표현, 네트워크 주소를 제외한 나머지 이다.

![https://user-images.githubusercontent.com/70089259/148165982-233d3c1e-166d-4c30-b417-fcaa76df54b0.png](https://user-images.githubusercontent.com/70089259/148165982-233d3c1e-166d-4c30-b417-fcaa76df54b0.png)

**NAT(network address translation) - 사설 주소**

공통 주소(공인IP)로 들어오고 나가지만, 내부에서 사용할 수 있도록 NAT라는 기계를 이용해서 사설IP로 바꾸어서 사용. 공인IP는 사설 IP를 가지지 못한다. 라우터로 봐도 된다.

같은 네트워크가 아니라면 사설 주소를 자유롭게 이용한다.

![https://user-images.githubusercontent.com/70089259/148169513-01eb1723-7088-4fca-a587-08b73e611b1e.png](https://user-images.githubusercontent.com/70089259/148169513-01eb1723-7088-4fca-a587-08b73e611b1e.png)

### 서브넷 마스크

classful address는 그대로 사용하기에 불편하고 낭비가 심하다.

기존의 방법을 깨버리는 서브넷팅이라는 방법이 나온다.

192.168.30.0이라는 네트워크는 호스트를 256 -2 개 가진다.

구간 : 192.168.30.0~192.168.30.255  → prefix : 24

prefix를 하나 씩 늘릴 때마다 호스트 수가 절반으로 줄면서 네트워크를 늘린다.

192.168.30.0~192.168.30.127

192.168.30.128~192.168.30.255

![https://user-images.githubusercontent.com/70089259/148172362-22b20c77-024f-4b1b-8a55-0b87ecb98070.png](https://user-images.githubusercontent.com/70089259/148172362-22b20c77-024f-4b1b-8a55-0b87ecb98070.png)

**정의**
네트워크를 분리 및 합치는 개념

**호스트 개수**

2^(32-prefix) - 2

**구간 구하기**

192.168.100.0/28

2^(32-28) = 16 - 2 = 14, 256/16 = 16

192.168.100.0 ~ 192.168.100.1s

192.168.100.16 ~ 192.168.100.31

### IP프로토콜

**비연결형 서비스**

상위 계층인 TCP 등에서 신뢰성을 보장해야한다.

**IP헤더**

32bit = 4byte = 1word → 총 5개의 word로 이루어져있다.

마지막줄은 필수가 아니고, IHL을 확인하여 유/무 파악 가능

![https://user-images.githubusercontent.com/70089259/148334158-3490fd38-34a1-4d0b-9ad5-92f14b963af3.png](https://user-images.githubusercontent.com/70089259/148334158-3490fd38-34a1-4d0b-9ad5-92f14b963af3.png)

version

IHL : 옵션 여부를 알 수 있다. 5word일 경우가 없음

TOS :  서비스 성능 측정

Total Length(header length + data length) : 20~1500의 범위의 값, 20은 헤더의 기리이

Identification(식별번호)

Flags(DF/MF) : DF는 패킷 분할 금지, MF는 0일 경우 마지막 패킷

Fragment Offset(순서번호) : 분할 전 상대적인 위치 정보, 8 바이트의 배수로 지정

TTL(패킷 생존 시간)

Protocol :TCP, UDP, ICMP 등

Header Checksum : IP에 관해서만 따진다

Source address : 송신지 IP 주소

destination address : 수신지 IP 주소

options + pad : 없을 수 있음

**프레임**

preamble(8) Destination Address(6) Source Address(6) Type(2) Data(0~1500) pad(0~46) checksum(4)

**IPv6**

16바이트, 128비트로 구성. 2바이트 8개 = 16바이트이다.

2바이트는 4개의 16진수로 표현

필드의 표기는 4개의 16진수로 표현

표기는 왼쪽 0은 생략가능 00ff → ff

연속된 필드가 0일 경우 ‘ :: ’ 으로 표기 가능하다.

**IPv4 IPv6 호환 방식**

이중 스택 : 헤더 2개 장착하기

터널링 : 특정 IP만 지날갈 수있는 네트워크의경우 특정 IP안에 집어넣어서 통과한다

헤더변환 : 수신 측에 알맞게 헤더 변환을 한다.

### 프로토콜 기초

정보프레임 I :  상위 계층이 전송 요구한 데이터를 송신하는 용도, 순서번호, 송수신 호스트 정보 등

긍정프레임 ACK : 전송한 데이터가 올바르게 송신되었는지

부정프레임 NAK : 전송 데이터가 오류가 있어서 재전송 요청하는 용도

**피기배킹**

정보프레임 긍정프레임 따로 따로 패킷을 보내면 복잡하므로 둘이 합쳐서 보내는 개념

표기법 : I(m, n)
