![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87fc72d1-cf95-4cf8-a2a0-72bd86c67a89/Untitled.png)

### Cluster 구성요소

**Control Plane이란?**

- Kubernetes Cluster를 제어하기 위한 서버 ( Master )

**Control Plane 구성요소**

- **API-server**
    - Cluster의 모든 구성 요소들이 Control Plane에 위치하는 API 서버를 거쳐 메시지를 주고 받음
    - 클러스터로 전달된 요청이 유효한지 검증함
- **etcd**
    - Key-value로 구성된 Kubernetes Cluster를 구성하는 데이터베이스
- **scheduler**
    - 새로 생성되는 Pod를 감지하고 적절한 노드에 배정하는 역할을 수행
- **controller-manager**
    - Controller는 API Server를 통해 Cluster의 상태를 모니터링 및  선언 상태로 유지함
    - Controller Manager는 Controller의 기능을 지원하기 위한 구성요소
- **cloud-controller-manager**
    - Kubernetes가 Cloud와 연동되기 위한 Controller Manager
    - Node Controller
    - Route Controller
    - Service Controller
    - Volume Controller

---

**Worker-Node란?**

- Container를 실행할 수 있는 컴퓨터

**Node-구성요소**

- **kubelet**
    - Kubernetes Cluster의 Agent 역할을 수행하며 각 노드에 존재하는 구성요소
    - 파드 및 컨테이너 실행 등을 직접 관리, Pod Spec에 따라 컨테이너를 실행하고 모니터링
- **kube-proxy**
    - Kubernetes Cluster의 각 노드의 네트워크 기능을 담당하는 구성요소
