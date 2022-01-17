### 리눅스 패키지 관리 도구

**rpm**

- 로컬에서 사용한다. exe파일과 흡사
- 의존성 문제가 있다. → 하위패키지가 있어야 상위패키지가 설치가 되는 상황
- 패키지 관리 용도로 사용
- 컴파일되어 설치할 실행,설정,라이브러리 등이 패키지화 되어 있다.
- **파일형식**  → 이름-버전-릴리즈 버전-아키텍처.rpm
- **옵션**
    - [-q] 정보확인
    - [-qa] 설치된 모든 패키지 확인
    - [-qa | grep httpd] 설치된 모든 패키지 중에 httpd키워드 목록
    - [noarch] python, perl같은 스크립트일 경우 아키텍처가 필요없다.
    - [-ivh] v정보확인, h압축풀기 파일 설치할 때 쓰는 키워드

**yum**

- 네트워크 연결 기반
- 자동으로 하위 패키지 설치가 되면서 의존성 문제 해결
- rpm 기반의 시스템을 위한 자동 업데이트 및 패키지 설치 제거도움
- **옵션**
    - [list | grep mariadb] 패키지 리스트 확인
    - [search keyword] 키워드에 따라 패키지 나열
    - [info httpd] 패키지 정보 확인
    - [provides 경로] 해당 패키지가 어떤 패키지와 관련있는지 확인
    - [-y install 패키지*] 패키지명으로 시작되는 모든 패키지가 설치되면서 모든 응답에 y표시
    - [update] 패키지 업데이트
    - [remove 패키지] 패키지 삭제
    - [group list 패키지] 해당 패키지가 포함되어 있는 그룹 확인 가능
    - [group info “Security Tools”] 패키지 그룹의 정보 확인
    - [group install “그룹명”] 해당 그룹 패키지 설치
    - [history] 실행 로그 출력

**#vim /etc/yum/repos.d/CentOS-Base.repo**

[base]

name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/

gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

- 확장자명은 repo로 끝난다.
- [id] - 아이디 지정, 임의 지정 가능
- name - 이름 지정, 시스템에서 인식(대부분 ID부분과 동일하다)
- mirrorlist - 중간 저장소
- baseurl - 실제 다운로드할 패키지의 원본 주소
- enabled - 직접 구성할 때(동작 여부 설정 0 or1, true or false)
    - 리포가 여러개 있을 경우 하나라도 동작하지 않으면 패키지 설치가 불가능하다. 그래서 잠깐 비활성화하는 용도로 사용한다. 위 예시는 시스템 구성이므로 생략
- gpgcheck - gpgkey 사용 여부 설정
- gpgkey - 패키지 설치 시 키를 자동으로 가져올 수 있음
    - 자동 업데이트 시 키를 사용
    - 라이센스와 같은 역할
    

### 리눅스 파일 시스템 엑세스

**파일 시스템**

해당 저장 공간에서 작업할 때 적용되는 옵션들을 설정해두는 것(압축, 공유, 작업 설정 등)

계층 구조랑도 관련이 있다.

**마운트란?**

장치를 디렉토리에 얹는 개념

파일 시스템을 적용한 장치에 디렉토리를 마운트할 수 있다.

mount [마운트할 대상][마운트 포인트]

**스토리지가 포함된 장치와 관련된 디렉토리 /dev**

lsblk(lsblock) - 스토리지 정보 확인

**리눅스의 파일 저장 방식**

장치에 직접 저장하지 않고, 디렉토리에 마운트를 하고 해당 디렉토리에 파일을 저장한다.

또한 파일 시스템은 장치에 마운트 되어있으므로, 해당 파일 시스템은 디렉토리에 마운트되어있다. 

파일 시스템이 장치에 마운트되어있지 않으면, 장치를 디렉토리에 마운트할 수 없다.

**디스크 파티션**

SATA-디스크 직렬 연결

디스크 파티셔닝을 위한 선행 작업 - 디스크 추가

- 새 저장소 연결 추가

**#df**

어떤 디렉토리에 마운트되어있는지와 해당 파일 시스템 옵션 저장된 파일 및 시스템 출력

 

**#du-h**

디렉토리나 사용자별 디스크 사용량 확인

### 작업 스케줄링

**at**

- 단일성 작업 예약도구
- 시스템 데몬 atd로 프로세스 제어
- 백그라운드에서 동작, 제어 터미널X
- 설치
    - yum -y install at
    - systemctl start atd
- 실행예제
    1. at now +1min
    2. echo “test” > test.txt
    3. ctrl + d <EOT>
- 기준 시간이 될 수 있는 문자열 예제
    - at teatime tomorrow 다음날 16:00
    - at noon +4 days 4일 뒤 정오
    - at 5pm august 3 2022
- 기타 옵션
    - at [-l] 예약된 명령어 확인
    - atrm 2 예약된 명령어 삭제

**주기적인 작업 예약**

- 예약된 작업을 반복 실행
- crond 데몬이 작업을 제어(crontab 구성 파일 해석)
- **#vim /etc/crontab**
    - crontab 파일확인
    - ***** root echo “test” > test.txt  → root 사용자가 매 분 마다 해당 명령어 실행
        - 분 시 일 달 요일
        - 01*** → 매일 새벽 1시 마다 실행
        - * 현재시간기준으로실행
        - 0/3 1 * * * (간격실행) → 매일 오전 1시에 3분 마다 실행하여 2시가되면 실행 x
    - 일반 사용자는 crontab -e 에서 반복 작업 생성
- **#vim /etc/cron.d/[주제]**
    - creat remove와 같은 주제에 알맞는 반복작업을 실행
- 기타 옵션
    - crontab [-l]  반복 작업 예약 확인
    - crontab [-r]  일반 사용자는 모든 반복 작업을 제거
- **#vim /etc/anacrontab**
    - 시스템이 종료되었거나 절전 중이라서실행을 못했을 경우 시스템이 가동된 시점을 기준으로 예약 작업 실행한다.
    1. 기간(일)
        - @daily 는 정수 1과 동일, 매일 실행됨을 의미
        - @weekly 는 정수 7과 동일, 매주 실행됨을 의미
        - @monthly 는 매달 실행됨을 의미(달마다 일 수가 다르기 때문)
    2. 지연(분)
        - 시스템이 준비된 후 작업을 시작하기 전에 crond 데몬이 대기해야
        하는 시간
    3. 작업 식별자
        - 작업이 식별되는 고유 이름
    4. 명령
        - 실행할 명령
