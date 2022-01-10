## Virtual box

**키워드** **옵션**

Nat - 하나의 네트워크에서 가상머신과 1대1 매칭

Nat network - 하나의 네트워크에서 복수의 가상머신 네트워크 사용, 가상머신과 외부를 연결해준다.

host-only Network - 내부 가상머신들끼리 통신 시 사용

**기본네트워크 설정**

- 파일
    - 환경설정 - NAT 네트워크 추가
    - 호스트 네트워크 관리자 추가

**가상머신 설정**

네트워크 설정 - 어댑터1:Nat network, 어댑터2:호스트 전용 어댑터

저장소에서 iso파일 넣기

**Putty와 연결**

1. [Host Name] - 192.168.56.10 입력
2. [Saved Sessions] - 192.168.56.10 입력
3. Open

**네트워크 설정 개념**

 

nmcli con add con-name ens1 type ethernet ifname enp0s3 \
ipv4.method manual ipv4.addresses 10.0.2.10/24 ipv4.gateway 10.0.2.1 \
ipv4.dns 8.8.8.8

nmcli con add con-name ens2 type ethernet ifname enp0s8 \
ipv4.method manual ipv4.addresses 192.168.56.10/24 ipv4.gateway 192.168.56.1 \
ipv4.dns 8.8.8.8

enp0s3 → ens1, ip 10.0.2.11, subnet mask 255.255.255.0, gateway 10.0.2.1, dns 8.8.8.8

- 내부(NatNetwork)
- 환경설정 네트워크에서 추가

enp0s8 → ens2, ip 192.168.56.11, subnet mask 255.255.255.0, gateway 192.168.56.1 dns 8.8.8.8

- 외부 통신
- virtual box 호스트 네트워크 관리자에서 설정

nmcli con up ens1

nmcli con up ens2

nmcli con show  # 제대로 설정 확인
