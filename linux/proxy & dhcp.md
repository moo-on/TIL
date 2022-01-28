### 리눅스 프록시 서버

**정의**

- 네트워크 중간에서 대리인 역할 가능한 서버
- 중간에서 데이터를 받아 전달하는 역할

**구축실습**

```bash
yum -y install squid

# 로컬에서 바로 웹을 접속하는데, 프록시를 거쳐서 웹을 접속하도록 해제
vim /etc/squid/squid.conf
	53: http_access allow all
	62: cache_dir ufs /var/spool/squid 1000 16 256  # 캐시서버 최대 1gb까지 저장 가능

# 방화벽 해제
```

### 리눅스 DHCP

**정의**

한정된 ip주소를 유용하게 사용 가능한, 자동으로 ip주소 할당해주는 프로토콜.

**프로세스**

1. DHCP Discover 브로캐스트로 서버 찾음
2. DHCP offer 가능한 ip 제안
3. DHCP Request 제안 ip 수락 요청
4. DHCP Ack 네트워크 정보와 함께 임대
- dhcp서버 구축 실습

```bash
ymu -y install dhcp

# 설치 확인 후 충돌 방지로 색인된 프로세스 종료
ps -ef |grep dnsmasq
kill -9 1538

vim /etc/dhcp/dhcpd.conf
'''
	subnet  10.0.2.0        netmask 255.255.255.0   {
        option  routers 10.0.2.1
        option  subnet-mask     255.255.255.0;
        range   dynamic-bootp   10.0.2.10       10.0.2.100;
        option  domain-name-servers     8.8.8.8;
        default-lease-time      10000;
        max-lease-time  50000;
}
'''

# 서비스 재시작 및 허용 방화벽 허용

```
