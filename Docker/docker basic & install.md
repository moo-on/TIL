### 도커를 알기 위한 기본 CS

**가상화(Virtualization)**

- 서버 가상화
    - 서버를 가상 머신의 형태로 만들어서 사용하는 기술
     CPU, RAM, 저장장치 등을 논리적으로 나눠 사용하는 기술
- 네트워크 가상화
    - 물리적으로 존재하는 네트워크를 논리적으로 구성하는 가상화 기
- 스토리지 가상화
    - 여러개의 물리적인 디스크를 하나의 논리적인 디스크 처럼 사용하는 기술
       최근에는 SDS(Software Defined Storage) 기술이 주목을 받고 있음 - Ceph, GlusterFS
- 컨테이너 가상화
    - 호스트 OS에서 논리적인 구역을 만들어 서로 독립적인 애플리케이션을 실행하는 기술
      커널의 기능을 사용하며 서버가상화보다 리소스를 효율적으로 사용할 수 있음

**Linux LXC**

**FreeBSD Jail**

- 프로세스 구획
- 네트워크 구획
- 파일시스템 구획

**Solaris Containers**

- Zone
- 리소스 관리

**cgroups**

- 프로세스, 쓰레드를 그룹으로 묶어서 관리하는 기능과 시스템의 리소스(CPU, RAM, Disk)에 대해 제한하는 기술

**Namespace**

- 각각의 오브젝트를 격리하기 위한 리눅스 커널 기술
- PID Namespace : 독립적인 PID 사용할 수 있는 Namespace
- Network Namespace : 독립적인 네트워크를 사용할 수 있는 Namepspace
- UID Namespace : 독립적인 UID를 사용할 수 있는 Namespace
- Mount Namespace : Mount Point를 독립적으로 사용할 수 있는 Namespace
- UTS Namespace : 독립적인 호스트네임을 사용할 수 있는  Namespace
- IPC Namespace : 독립적인 IPC를 사용할 수 있는 Namespace

**Union Filesystem**

- 하나의 파일시스템이 아닌 각각 독립적인 여러개의 파일시스템 레이어가 묶여서 하나의 파일시스템으로 이용할 수 있도록 하는 파일시스템 기술

**Docker 주요 용어 정리**

- 컨테이너 이미지
    - 컨테이너의 애플리케이션 및 애플리케이션이 실행하기 위한 라이브러리 등을 포함하는 단위로 컨테이너를 실행하기 위해 반드시 필요함
- 컨테이너
    - 컨테이너 이미지가 실행되는 형태로 컨테이너 이미지가 메모리에 로드 되면 컨테이너가 실행됨
     애플리케이션, 라이브러리, 컨테이너 실행 데이터를 포함함
     일반적으로 하나의 애플리케이션을 포함하고 실행함
- 레지스트리(저장소)
    - 컨테이너 이미지가 저장되는 저장소
     대표적인 도커 레지스트리는 도커 허브
- 레포지토리(Repository
    - 컨테이너 이미지가 업로드 된 공간
     일반적으로 "Docker_Hub_계정/이미지_이름:태그"
     Official Imge는 Docker Hub 계정이 생략됨

**도커설치**

```bash
yum install epel-release

cd /etc/yum.repos.d/

wget https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce.repo

yum install docker-ce docker-ce-cli containerd.io
systemctl restart docker.service
systemctl enable docker.service

# /etc/group에서 원하는 유저를 도커사용 가능하게끔 한다.
usermod -aG docker {user} # 재접속필요 # source ~/.bashrc
```
