### 쿠버네티스 오브젝트 관리

1. 명령형 커맨드
    - kubectl 명령어에 인수 또는 옵션을 지정하여 애플리케이션을 관리
    - 일회성 작업에서 주로 사용
2. 명령형 오브젝트 구성
    - 오브젝트를 Manifest File로 정의함
    - kubectl 명령어에서 YAML이나 JSON 파일을 인수로 사용하여 오브젝트를 관리함
    
    ```bash
    kubectl create -f MANIFEST
    kubectl delete -f MANIFEST
    ```
    
3. 선언형 오브젝트 구성
    - 특정 디렉토리에 모든 오브젝트 파일을 배치하여 관리함
    - kubectl 명령어에서 디렉토리를 인수로 하여 오브젝트를 관리함
    
    ```bash
    kubectl create -f DIRECTORY
    ```
    

**오브젝트 종류**

- Pod
- ReplicationController
- Replicaset
- Volume
- Configmap
- Secret

### 명령어를 통한 오브젝트 생성과 서비스 생성 후 실행

```bash
# 오브젝트(파드) 생성
kubectl run nginx --image nginx:1.14
kubectl get pods

# 오브젝트에 접근 가능하도록 네트워크 서비스 생성
kubectl expose pods nginx --port=80 --protocol=TCP --name test-svc --type=NodePort

# 네트워크 서비스 확인
kubectl get services

# 확인된 아이피로 네트워크 접속
curl http://10.98.155.131

# 서비스 삭제
kubectl delete service test-svc

# 오브젝트 삭제
kubectl delete pod nginx
```

### Manifest File을 이용하여 오브젝트 생성

```bash
vim hello-nginx.yaml
---
apiVersion: v1
kind: Pod
metadata:
	name: hello-pod
spec:
	containers:
		- name: nginx-hello
			image: nginx:1.14
			ports:
				-name: web-port
				 containerPort: 80
				 protocol: TCP
---

kubectl create -f hello-nginx.yaml
kubectl get pods

```

**상세정보**

```bash
kubectl describe OBJECT NAME ex)describe pod test-pod
```
