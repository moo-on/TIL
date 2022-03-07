**가동중인 컨테이너 연결**

```bash
# 가동 컨테이너에 연결
docker container attach CONTAINER

# 가동 컨테이너에 프로세스 실행
docker container exec [option] CONTAINER
-d(백그라운드), -i(표준입력), -t(tty단말디바이스), -u(사용자명)

# 가동 중인 컨테이너에 실핼중인 프로세스 확인
docker container top CONTAINER

# 가동 컨테이너의 포트 전송 확인
docker container port CONTAINER

# 가동 컨테이너 이름 변경
docker container rename old new

```

**컨테이너의 파일 이동 및 변경**

```bash
# 도커 호스트에서 컨테이너로 파일 복사
$ docker container cp DOCKER_HOST_PATH CONTAINER:/CONTAINER/PATH

# 컨테이너에서 도커 호스트로 파일 복사
$ docker container cp CONTAINER:/CONTAINER/PATH DOCKER_HOST_PATH

# 컨테이너 조작 차분 확인
$ docker container diff CONTAINER

<DIFF 상태>
 A  파일 추가
 C  파일 수정
 D  파일 삭제
```
