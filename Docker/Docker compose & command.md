### volume mount(-v) 명령어

```bash
docker stop mysql
docker rm mysql
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --network=app-network \
  --name mysql \
  -v /my/own/datadir:/var/lib/mysql \
  mysql:5.7

#  연결된 DB를 삭제하고 다시 연결하면 기존에 DB는 사라진다. 기존 데이터를 저장하기 위한 명령어
```

### compose 명령어

```bash
docker-compose 생략
docker-compose up  # docker-compose.yml파일을 읽어서 한번에 실행한다.
start  # 멈춘 컨테이너 재개, 전체 혹은 특정
restart  # 컨테이너 재시작, 전체 혹은 특정
stop  # 컨테이너 멈춤, 전체 혹은 특정
down  # 컨테이너 종료 후 삭제
logs  # 컨테이너의 로그 -f옵션
ps  # 컨테이너 목록
exec  # 실행중인 컨테이너에서 명령어 실행
build  # 빌드로 선언된 컨테이너만 빌드 되거나, 특정 컨테이너를 선택
```

### docker-compose.yml 구성

```bash
version: yml파일의 버전
sevices:
	name: 컨테이너의 이름 정의
		image(build): 사용할 이미지의 이름과 태그, 생략시 latest
		volumes: 마운트할 디렉터리들
		restart: 재시작 정책
		environment: 환경변수들
		ports: 컨테이너와 연결할 포트들

```
