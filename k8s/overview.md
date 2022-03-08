# 쿠버네티스 overview

**쿠버네티스 사용 이유**

전체 서비스의 자원 할당을 하나로 묶을 수 있어서, 효과적이다.

![image](https://user-images.githubusercontent.com/70089259/157208934-d5066386-5139-4eef-a080-c4836207f750.png)

**overveiw**

- 하나의 서버가 마스터이며, 여러개의 노드가 연결 되있다.
- 노드들을 추가하면 클러스터 자원이 증가한다.
- 클러스트 안에 NameSpace를 통해 독립적 공간으로 만든다.
- NameSpace안에 쿠버네티스 최소 배포 단위인 Pod들이 있다.
- 해당 pod들 외부 연결이 가능한 service가 있으며, 서로 다른 name space에는 연결이 불가능하다.
- pod 안에 여러 컨테이너들이 있기에, pod안에는 여러 컨테이너가 동작 가능하다.
- pod가 날라가면 데이터가 날라가기에 따로 volume을 만들어서 pod에 연결해서 사용한다.
- namespace에 자원의 양을 제한할 수 있다. → resourceQuota limitRange
- 환경변수 및 파일 마운트 가능 → configMap secret
- 용도에 맞는 **컨트롤러**가 pod들을 관리해준다.
    - Replication Controller, ReplicaSet → pod가 죽거나  autoscaling해준다.
    - Deployment → 배포 후에 pod들을 업그레이드 해주며, 과정에 문제가 있다면 롤백한다.
    - DemonSet → 한 노드에 하나의 pod만 사용하게끔
    - Cronjob → 특정 작업을 하고 종료하는 경우.
