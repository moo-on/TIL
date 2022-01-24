**키워드 정리**

호스트와 도메인 : www/naver.com

FQDN : 전체 도메인 네임

URL : 특정 웹사이트의 특정 위치까지 가는 경우

### DNS 설정

**window**

- nslookup
    - DNS확인
- set type=DNS레코드이름(ns, mx, all)
- windows/system32/drivers/etc/hosts
    - 해당 파일에서 관리
    

**linux**

- dig @8.8.8.8 [ns, a] naver.com
- host [naver.com](http://naver.com) [8.8.8.8]
    - 뒤에 DNS주소를 입력 시 해당 DNS 서버로 가서 질의를 한다.
- etc/hosts
    - 해당 파일에서 관리
- 현재 DNS 설정 값 확인 /etc/resolv.conf
    - 해당 방식은 과거, 현재 NetworkManager로 설정

**ip반환 과정**

- host → DNS cash → 시스템에 설정되어 있는 DNS 서버
- DNS 서버 → root DNS 서버 → 1차 DNS서버(com 최상위 네임 서버) → naver.com의 네임서버 → ip를 알려준다.

**SELinux : 리눅스 보안커널**

- 강제 : enforcing
- 허용 : permissive, setenforce 0
- 비활성 : disabled
- 영구 설정 vi /etc/sysconfig.selinux
    - selinux = disabled 설정

**캐시 전용 DNS 서버 구축**

- yum -y install bind bind-chroot
- 설정변경 /etc/named.conf
    - listen-on port 53 { 127.0.0.1; }; → any → 열어주기
    - listen-on-v6 port 53 { ::1; }; → none →ip6사용x
    - allow-query     { any; }; → any → dns쿼리 주고 받을 때 사용
    - 변경 후
        - systemctl restart named
        - systemctl enable named
- 방화벽 허용
    - firewall-cmd —permanent —add-service=dns
        - firewall-cmd —permanent —add-service=53/tcp(udp) 으로 대체 사용 가능
    - firewall-cmd —reload
    

10.0.2.6  → 서버

10.0.2.7 → 서버2

무조건 NAT네트워크로 가상머신들끼리 통신, host에서 가상머신 접속 원할경우 호스트 네트워크 추가

**서버1 구성**

- rpm -qa httpd
- systemctl restart httpd
- firewall-cmd —permanent —add-service=http
- firewall-cmd —reload
- cd /var/www/html
    - index.html 추가
- zone transfer(영역전송) 허가 → **/etc/named.conf**
    - zone "[linux.com](http://linux.com/)" IN {
    type master; 
    file "linux.com.db";
    allow-update { none; };
    };
    - 메인 dns서버 → 마스터
    - db라는 상세정보파일 만들기
    - 오타 체크 named-checkconf
- cd /var/named
- 기본 로컬호스트파일을 수정하는 것이 더 편하다, 양식 가져오기
    - cp named.localhost linux.com.db
    - **/var/named/linux.com.db**
    - @와.는 [linux.com](http://linux.com) 이라는 뜻
        
        $TTL 1D
        @       IN SOA  @ root. ( # @(linux.com)에 권한(SOA)을 가지고 있는 자는 linux.com이다.
        0       ; serial
        1D      ; refresh
        1H      ; retry
        1W      ; expire
        3H )    ; minimum
                 NS      @   # 빈칸은 linux.com, 리눅스닷컴의 네임서버는 리눅스닷컴이다.
                  A       10.0.2.4
        www  A       10.0.2.4
        ftp     A       10.0.2.8
        
        - 109pg 참조.
        
        named-checkzone [linux.com](http://linux.com) linux.com.db
        
        chmod -R 754 /var/named
        
        systemctl restart named
        
        dig @localhost www.linux.com
        
- 확인
    - window에서 dns버 주소 해당 서버로 바꿔주고 linux.com입력
    - nslookup → 10.0.2.6
        
        

서버2

yum -y install vsftpd

systemctl restart vsftpd

firewall-cmd —permanent —add-service=ftp

firewall-cmd —reload
