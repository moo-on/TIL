**방화벽이란**

- 1차적인 보안 수단
- 패킷제어를 함으로서 네트워크로 침투하는 경로의 침투 보완
- 과거 iptables, 7버전 이후 firewalld 사용
- 설정 방법 3가지
    - 그래픽 도구 - firewalld config, cockpit
    - 명령어 도구 - firewall-cmd
    - 설정파일

**firewalld-cmd** 

- 현재 구동 상태에 즉시 적용
- 기본정책 : 거부, 설정을 통해 허용해준다.
- —permanent 영구 설정 옵션
- #firewall-cmd —reload
    - 재부팅 시 예전 정책 초기화 → 영구 설정 필요
- firewall-cmd —list-all
    - 설정 확인
    - -zones
    - —permanent
- firewall-cmd —add(remove)-service=ftp
    - 서비스 허용 및 제거
- firewall-cmd —add-port = 21/tcp
- firewall-cmd —add-port = 8000-9000 /tcp
    - 포트 허용 룰 추가
    - 21번(ftp) 포트 허용
- firewall-cmd —add-source = 192.168.56.0/24
    - 해당 대역 허용
- firewall-cmd —add-source = 192.168.56.11/32
    - 특정 아이피 허용

**firewalld zone**

- zone이라는 영역 개념이 존재하여 인터페이스 별로 영역을 설정하여 관리를 쉽게 할 수 있다.
- firewall-cmd —get-zones → 사전 정의된 존
- firewall-cmd —get-active-zones →활성화된 존 정의
- firewall-cmd —get-default-zones → 전체 존 정의
- firewall-cmd —list-all-zones → default로 정의된 존

- firewall-cmd —new-zone=webserver —permanent  → 웹서버라는 존 생성
- firewall-cmd —delete-zone=webserver —permanent  → 웹서버라는 존 삭제

- firewall-cmd —zone=webserver —add-service=http —permanent   ,remove가능

- firewall-cmd —set-default-zone=webserver
- firewall-cmd —set-default-zone=public

**rich-rule**

특정 아이피만 차단

- firewall-cmd -add-rich-rule=’rule family=”ipv4” source address=192.168.56.100 reject’
    - add 대신 remove
    - reject 거절 메시지
    - drop 그냥 버리기
    

**Linux 접근 통제 방식(2가지)**

- DAC(Discretionary Access Control, 임의적 접근 통제) 모델
    - 접근하려고 하는 사용자가 누구인지 확인하여 접근 제어
    - 일반적인 시스템에서 사용
    
- MAC모델
    - Mandatory Access Control, 강제적 접근 통제
    - root 사용자에게 막강한 권력이 있어서
    - 지속적으로 권한 탈취 문제가 야기, 관리자의 실수로 인해 다양한 장애 등이 발생할 수 있는 위험에 노출되어 있음
- 일반 사용자에게 제한적인 권한 이외에도 쉘 또는 웹 등으로만 접근할 수 있게하는 등, 프로세스로만 접근 가능하다.

**SELinux(Security Enhanced Linux)**

- SELinux라는 중요한 보안 기능을 제공
- 안전하게 사용하기 위함
- 모든 파일, 프로세스, 디렉토리, 포트에는 특수 보안 레이어가 있음
- 특수 보안 레이블에는 여러 컨텍스트가 존재
    - 컨텍스트 종류 : 사용자 역할 유형 민감도
- 활성화되는 기본 정책인 타겟 정책은 유형 컨텍스트를 기준으로 정해진다.
    - 이름은 주로 _t로 끝난다.
- ex)
    - 웹서버의 유형 컨텍스트는 httpd_t
        - /var/www/html에서 주로 발견되는 파일 및 디렉토리의 유형 컨텍스트는 httpd_sys_content_t
    - mariaDB 서버의 유형 컨텍스트는 mysqld_t
        - /data/mysql에서 주로 발견되는 파일 및 디렉토리의 유형 컨텍스트는 mysqld_db_t

**프로세스** **유형 컨텍스트확인**

ps axZ, Z가 유형 컨텍스트

**특정 컨텍스트 확인**

systemctl start httpd

ps -ZC httpd

ls -Z /var/www

- cp를 통해 이동 시 같은 파일을 복사하더라도 위치가 바뀌면 컨텍스트 유형이 바뀐다.
- a옵션 사용하여 복사하면 파일의 유형 컨텍스트가 바뀌지 않는다.

**SE Linux** **정상 동작확인 및 설정**

getenforce

setenforce 0(1)  → 활성화 및 비활성화

**SELinux 설정 파일**

#vim /etc/selinux/config

- setenforce를 통한 런타임 설정 가능
    - enforcing → SELinux 정책 강제
    - permissive → 정책 위반시 차단이 아니라 로그에 기록
- 파일에서 설정 후 재부팅
    - disabled → SELinux 비활성화
    

**SELinux 부울**

- 추가적인 기능 설정, 스위치 처럼 on, off
- semanage boolean -l , getsebool -a  → 불 설정 확인, 후자는 현재 상태만 확인 가능
- setsebool httpd_enable_homedirs on  → 불 설정 켜기, 현재 상태만
- setsebool -P httpd_enable_homedirs on  → 영구 설정 변경, semange로 확인해야함
- semanage boolean -m -0 httpd_enable_homedirs → 설정 한번에 변경

**semanage(보안 정책 설정 및 확인)**

- [-a][-t] t옵션으로 추가
- semanage fcontext -l | grep /var/www
    - 디렉토리에 적용되어 있는 유형 컨텍스트 확인(시스템에 등록)
- 유형 컨텍스트 등록 후에 적용
    - restorecon -RFv /context
    - R : 하위 디렉토리에 적용
    - F : 콘텍스트 재설정
    - v : 정보 출력
- ex1)
    - semanage fcontext -a -t httpd_sys_content_t ‘/context(/.*)?’
    - context 폴더 밑에 httpd_sys_content_t 유형을 설정

**임시적으로 정책 변경**

chcon -t [default_t /content]

- 런타임 설정

vim /etc/httpd/conf/httpd.conf → #listen에서 포트 번호 변경

systemctl restart httpd → 서비스 재시작

semanage port -a(d, m) -t http_port_t -p tcp 82 → 포트 등록 삭제 타입 수정(포트바인딩변경)

firewall-cmd —add -t http_port_t -p tcp 82→ 방화벽 설정

해당 포트로 접속시 접속 가능
