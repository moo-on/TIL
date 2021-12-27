### ps 명령어

```bash
docker ps # 현재 실행 중인 컨테이너
docker ps -a # 중지된 컨테이너까지 전부
```

### stop 명령어

```bash
docker stop [OPTIONS] CONTAINER [CONTAINER...] # 실행중 컨테이너 중지, 복수 개 가능
```

### rm 명령어

```bash
docker rm [OPTIONS] CONTAINER [CONTAINER..] # 종료된 컨테이너까지 제거
```

### logs 명령어

```bash
docker logs [OPTIONS] CONTAINER 
# 컨테이너 정상 동작하는지 확인을 위해 로그를 확인
# -f 로그생성 대기
# 
```

### images 명령어

```bash
docker images [OPTIONS] [REPOSITORY[:TAG]]  # 도커가 다운로드한 이미지 목록을 본다.
```

### pull 명령어

```bash
docker pull [OPTIONS] NAME[:TAG|@DIGEST]  # 이미지 다운로드 명령어
```

### rmi 명령어

```bash
image 삭제 명령어
```

### network create 명령어

```bash
docker network create [OPTIONS] NETWORK
docker network connect app-network mysql
# mysql wordpress 연결 시 임의의 inner 호스팅이 아닌, 네트워크를 통해 관리하기 쉽다.
```
