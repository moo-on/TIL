### 도커 네트워크

 

**도커 네트워크 종류**

```
 Bridge Network
  - Docker 내부에 네트워크를 구성하고 호스트의 인터페이스를 사용해서 외부 네트워크와 연결될 수 있는 네트워크
  - Docker Host의 기본 브리지 네트워크 인터페이스 : docker0
  - Docker Container의 기본 브리지 인터페이스 : vethXXXX

 Host Network
  - Docker Host의 네트워크를 직접 공유하는 네트워크
  - Docker Host의 주소를 사용하여 Docker 컨테이너에서 사용중인 포트로 접근할 수 있음

 Null Network
  - 네트워크가 필요하지 않은 도커 컨테이너에서 사용하는 네트워크 유형으로 외부와 단절되는 네트워크 유형

 MAC VLAN Network
  - 호스트 네트워크 인터페이스와 같은 네트워크에 연결하는 네트워크 유형
```

**도커 네트워크 관련 주요커맨드**

```bash

#도커 네트워크 목록 확인
$ docker network ls
-f, --no-trunc, -q

#도커 네트워크 세부 정보 확인
$ docker network inspect NETWORK

#네트워크 지정하여 컨테이너 실행
$ docker container run -d -t --network NETWORK --name CONTAINER IMAGE

#도커 네트워크 생성
$ docker network create NETWORK -d NET_TYPE
-d, --ip-range, --subnet, --ipv6, -label, --internal
https://docs.docker.com/engine/reference/commandline/network_create/

#도커 네트워크 연결
docker network connet [option] NETWORK CONTAINER
--ip --ip6 --alias --link

```

```bash

```
