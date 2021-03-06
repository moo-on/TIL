## 서버를 관리한다는 것

- 새로운 설치 프로그램들
- 계속해서 바뀌는 서버 환경 & 개발 환경

도커를 통해 쉽게 관리하자

**간단한 실행**

docker-compose up

CLI로 간단하게 저장한 이미지를 실행한다. 

**전통적인 서버관리 방식**

Add user → system Env → Firewall →network → dependencies → python → git clone → package → configuration → migration → proxy → run

**도커에서는?**

어떤 프로그램이던 컨테이너화 하자

가상머신처럼 독립적이고,

가상머신보다 빠르고 쉽고 효율적입니다.

## 과거 서버를 운영하기 위한 노력

1. 문서화(ppt txt 등) 
2. 상태관리 도구(코드로 관리, 러닝커브, 다양한 버전 설치)
3. 가상머신(여러개 설치 쉽고, 현재 상태 저장, 초기 셋팅의 어려움, 서버 이미지 공유가 힘듬, 느림)
4. 자원격리(프로세스, 파일/디렉토리, CPU/메모리/IO 그룹별 제한 → 리눅스 기능을 이용한 효율적 서버관리) → 너무 어렵다

## **도커의 등장**

**개념**

격리된 환경에서 작동하는 프로세스

리눅스 커널의 여러 기술 활용

하드웨어 가상화 보다 가볍다

이미지 단위로 프로세스 실행 환경 구성

**주요 특징**

도커가 설치되어있다면 어디서든 컨테이너 실행 가능

배포 뿐만 아니라 쉽게 개발서버, 테스트서버를 만들 수 있다.

**표준성**

다양한 언어로 만든 프로젝트는 배포 방식이 다 다른데, 도커를 통해 동일하게 사용

**이미지**

이미지에서 컨테이너를 생성하기 때문에 이미지를 만들어야한다.

Dockerfile을 이용하여 만들고 처음부터 재현 가능하다

주로 빌드 서버에서 이미지를 만들어 저장하면, 운영 서버에서 불러와서 사용한다 ****

**설정관리**

설정은 주로 환경변수로 제어, 하나의 이미지에서 환경변수에 따라 동적으로 설정파일을 생성

**자원관리**

가상의 공간에 프로세스를 생성하는 것이기 때문에 컨테이너 삭제시 같이 사라질 수 있으므로 파일 혹은 캐시를 별도의 저장소에 저장해야한다. 

## 컨테이너의 미래

**발전**

도커란 하나의 프로그램을 관리하는 방식

여러대의 서버와 여러개의 서비스를 관리를 하려면? → 쿠버네티스

**스케줄링**

여러개의 서버 중에서 여유있는 서버를 찾아서 컨테이너를 배포해준다.

죽은 서버도 다른 서버로 넘길수 있음

**클러스터링**

10개의 서버를 하나의 서버처럼 사용하자

중앙 서버로 전부 관리, 가상 네트워크를 이용해서 마치 같은 서버에 있는 것 처럼 쉽게 통신

**서비스 디스커버리**

서로 다른 서버에 있는 컨테이너를 쉽게 찾는다.
