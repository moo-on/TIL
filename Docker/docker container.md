```bash
# 도커 버전
docker version

# 도커 이미지 확인 및 허브 이미지 확인
docker images
docker search {image_name}
# 이미지 다운로드
$ docker image pull IMG_REPO:TAG
$ docker pull IMG_REPO:T
# 도커 이미지 삭제
$ docker image rm IMAGE
$ docker image prune # 전체
$ docker image prune -a # 사용중이지 않은 이미지

# 도커 정보 확인 server 정보가 중요하다.
docker info

#CentOS 7  컨테이너 실행(제어 터미널 사용)                                                                                              
$ docker container run  -it  --name CONTAINER IMG_REPOSITORY:TAG
$ docker container run -it --name centos7_1 centos:7
# 컨테이너 실행(백그라운드)
$ docker container run -d --name CONTAINER IMG_REPO:TAG
$ docker run -d --name CONTAINER IMG_REPO:TAG

# Docker 컨테이너 목록 확인
$ docker container ls
$ docker ps
# 종료된 컨테이너 목록 확인
$ docker container ls -a
$ docker ps -a
# 실행 중인 컨테이너 종료
$ docker container ls # 컨테이너 아이디 확인
$ docker container stop { CONTAINER_NAME | CONTAINER_ID }

# 컨테이너에서 프로세스 실행(제어 터미널 사용)
$ docker exec -it CONTAINER COMMAND
-i	키보드 입력을 컨테이너의 표준 입력에 연결하여 키보드 입력을 컨테이너의 쉘 등에 보낸다
-t	터미널을 통해 대화형 조작이 가능하게 한다

#실행중인 컨테이너에 attach
$ docker container attach CONTAINER
$ docker attach CONTAINER

실행중인 컨테이너 종료
$ docker container stop CONTAINER
실행중인 컨테이너 강제 종료
$ docker container kill CONTAINER
컨테이너 삭제
$ docker container rm CONTAINER

컨테이너 로그 확인
$ docker container logs CONTAINER
도커 컨테이너 상태 확인(CPU, RAM, Network)
$ docker container stats CONTAINER

도커 컨테이너 일시 정지
$ docker container pause CONTAINER
도커 컨테이너 일시 정지 해제
$ docker container unpause CONTAINER

```

```jsx

컨테이너 상세 정보 확인
$ docker container inspect CONTAINER

컨테이너 이미지 상세 정보 확인
$ docker image inspect IMAGE

컨테이너 리소스 사용 정보 확인
$ docker container stats CONTAINER

컨테이너에서 실행중인 프로세스 확인
$ docker container top CONTAINER

```
