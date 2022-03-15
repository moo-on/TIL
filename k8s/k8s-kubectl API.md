**상태 확인**

```bash
kubectl SUBCOMMAND [RESOURCE] [ARG]
kubectl cluster-info # 클러스터 정보
kubectl get nodes # 각 노드 상태
```

**k8s-api**

- alpha, beta, stable(vN)
- 

**kubectl api-versions # cluster에 지원되는 버전 확인**

```bash
# apiVersion 설명

**v1**

쿠버네티스에서 발행한 첫 stable release API

(대부분의 api가 포함되어 있음)

**apps/v1**

쿠버네티스의 common API 모음, Deployment, RollingUpdate, ReplicaSet을 포함

**autoscaling/v1**

pod의 autoscale 기능을 포함하는 API, 현재는 CPU metric을 사용한 scaling만 가능

(추후에 alpha, beta version에서 memory, custom metric으로 scaling 기능 추가예정)

**batch/v1**

배치 프로세스, job-like task를 위한 배포 api

**batch/v1beta1**

batch/v1에서 cronJob으로 job을 돌리는 api가 추가

**[certivicates.k8s.io/v1](https://honggg0801.tistory.com/certivicates.k8s.io/v1) beta**

클러스터의 secure network function들이 추가된 API

(TLS 등의 기능 추가)

**extensions/v1beta**

Deployments, DaemonSets, ReplicatSets, Ingress 등 상당수 feature들이 새롭게 정의된 API

그러나 상당수의 api들이 apps/v1과 같은 그룹으로 이동되어서, 쿠버네티스 1.6버젼 이후부터는 deprecated 됨

**policy/v1beta1**

pod에 대한 security rule이 정의된 API
```

**kubectl api-resources # cluster에 지원되는 오브젝트 종류 및 버전 확인**

```bash
api resource의 약어가 무엇인지,
api resource가 namespace 기반 resource인지, 
사용할 수 있는 verb(권한)은 무엇이 있는지
```
