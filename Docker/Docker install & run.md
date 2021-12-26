### 도커의 설치

도커는 linux를 지원하기에 다른 운영체제에서 설치되는 Docker는 가상머신에 설치된다.

### 클라이언트 - 서버 구조

Docker CLI는 도커 호스트에게 명령을 전달하고 host에서 결과를 받아서 출력한다.

### 컨테이너 실행 - run

docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]

**OPTION에 들어가는 명령어**

| -d | 백그라운드 모드 |
| --- | --- |
| -p | 호스트와 컨테이너의 포트 연결 |
| -v | 호스트와 컨테이너의 디렉토리 연결 |
| -e | 컨테이너 내에서 사용할 환경변수 설정 |
| —name | 컨테이너 이름 설정 |
| —rm | 프로세스 종료시 컨테이너 자동 제거 |
| -it | -i -t 동시 사용으로 터미널 입력을 위한 옵션 |
| —network | 네트워크 연결 |

### 컨테이너 만들기 - ubuntu 20.04

```bash
docker run ubuntu:20.04
```

해당 이미지가 없다면, 다운로드(pull) 한 후 컨테이너를 생성 후 시작합니다.

컨테이너는 정상적으로 실행하고 명령어를 전달하지 않았기에 생성 즉시 종료합니다.

컨테이너는 프로세스 이므로 실행중인 프로세스가 없다면 종료하기 때문입니다.

### bin/sh 실행하기

```bash
docker run --rm -it ubuntu:20.04 /bin/sh
```

터미널 입력을 위한 -it

프로세스 종료 후 자동 삭제 -rm

/bin/sh 커맨드를 통한 쉘 스크립트 진입

exit명령어로 나갈 시 rm명령어로 자동 삭제된다. 원래는 스탑되고 그대로 남아있다고 한다.

### 컨테이너 만들기 - centos:8

```bash
docker run --rm -it centos:8 /bin/sh
```

도커는 다양한 리눅스 배포판을 실행하고, 동일한 커널을 사용한다.

여러 기능이 필요없는 운영체제를 사용할 경우 AIpine(5MB) 사용한다.

### 웹 어플리케이션 실행하기

```bash
docker run --rm -p 5678:5678 hashicorp/http-echo -text="hello world"

ex) 포트 접속
curl [localhost:5678](http://localhost:5678) → 으로 CLI로 url접속하여 출력 값 얻는다.
```

port 5678번에 hashicorp가 제공하는 5678번 포트에 연결한다.

연결하면, 지정된 텍스트 출력

### Redis 실행하기

```bash
docker run —rm -p 1234:6379 redis

ex) 포트 접속
telnet localhost 1234

set hello world

```

### MySQL 실행

```bash
실행
docker run -d -p 3306:3306 \
-e MYSQL_ALLOW_EMPTY_PASSWORD=true \
--name mysql \
mysql:5.7

접속
docker exec -it mysql mysql -> mysql에 접속하여  mysql명령어를 입력한다

```

### exec명령어의 활용

run 명령어와 다르게 실행중인 컨테이너에 접속한다.

도커는 다양항 DB의 생성과 삭제가 손쉽기에, 개발할 때 많이 사용한다.

### 워드프레스 블로그 실행

```bash
docker run -d -p 8080:80 \
  -e WORDPRESS_DB_HOST=host.docker.internal \
  -e WORDPRESS_DB_NAME=wp \
  -e WORDPRESS_DB_USER=wp \
  -e WORDPRESS_DB_PASSWORD=wp \
  wordpress
```
