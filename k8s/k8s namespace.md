**namespace란?**

k8s cluster를 논리적으로 나누는 구획

**namespace 종류**

- default
    - 기본 네임스페이스
- kube-system
    - 클러스터 핵심 리소스가 위치함
- kube-public
    - 모든 사용자가 읽기 권한으로 접근 가능하다.
- kube-node-lease
    - cluster nodes 가용성 점검을 위한 네임스페이스
    

**네임스페이스 확인 및 생성**

```bash

# 확인
kubectl get namespaces
kubectl get pods --all-namespaces
kubectl get pods -n NAMESPACE

# 네임스페이스 생성
kubectl create namespace NAMESPACE # 명령어
kubectl create -f NAME.yaml # 선언파일 
---
apiVersion: v1
kind: Namespace
metadata:
	name: test
---

# 원하는 네임스페이스에 pod 생성
---
apiVersion: v1
kind: Pod
metadata:
  name: kubernetes-simple-pod
	namespace: NAMESPACE # 해당 부분
  labels:
    app: kubernetes-simple-pod
spec:
  containers:
  - name: kubernetes-simple-pod
    image: arisu1000/simple-container-app
    ports:
    - containerPort: 8080
---

```
