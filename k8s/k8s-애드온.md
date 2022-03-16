**애드온이란?**

- Kubernetes Cluster는 크게 Master Node, Worker Node로 구성되지만 추가로 Addon 도 존재함
    
    Addon 은 Kubernetes Cluster 에서 기능을 구현 및 확장하는 역할을 담당함
    
    Kubernetes Cluster가 필요한 기능을 실행하기 위해 pod와 service 형태로 존재함
    
    Addon이 사용하는 Namespace 는 kube-system이며
    
    Addon에 사용되는 Pod는 Deployment, Replication controlller 등에 의해 관리됨.
    
    외부 연동 라이브러리라고 보면 됨.
    
- 애드온은 클러스터 내부에서 필요한 기능들을 위해 실행되는 포드들입니다. 애드온에 사용되는 포드들은 디플로이먼트, 리플리케이션컨트롤러등에 의해 관리됩니다. 애드온이 사용하는 네임스페이스는 kube-system입니다.

**애드온 종류**

- **네트워킹 Addon (CNI)**
    
    **CNI : Container Network Interface**
    
    ```bash
    컨테이너 간의 네트워킹을 제어할 수 있는 플러그인을 만들기 위한 표준
    
    Kubernetes Cluster 내부는 Master Node에 의해 여러 컨테이너가 생성 삭제 복구를 반복하고 있음
    
    그에 따라 각 컨테이너의 고정적이지 않고 재할당이 빈번함
    
    이러한 특징을 해결하기 위해 Kubernetes Cluster는 가상 네트워크가 구성되어 있는데
    
    기본적으로는 Worker Node의 kube-proxy 가 네트워크를 관리하지만
    
    보다 효율적인 네트워크 환경을 구성하기 위해
    
    다양한 네트워크 관련 Addon 이 제공됨
    ```
    
- **DNS Addon**
    
    다른 Addon 은 필수적이지 않지만 DNS Addon 만큼은 필수적임.
    
    실제로 Kubernetes Cluster 내에서 작동하는 DNS Server.
    
    Kubernetes Service Object에게 DNS 레코드를 제공하는 역할을 수행함.
    
    kubernetes 내부에서 실행된 컨테이너들은 자동으로 DNS 서버에 등록되어 자동 탐색이 가능해짐
    
    **종류 :** CoreDNS, kube-dns
    
- **대시보드 Addon**
    
    kubernetes Cluster를 위한 Web 기반 UI.
    
    일반적으로 kubernetse Cluster에 명령을 내릴 때 CLI에서 kubectl을 사용하는데
    
    가시화하여 관리의 편의성을 제공하기 위해 대시보드를 제공함
    
    **종류 :** kubernetes Dashboard, Weave Scope, NexClipper(국산),
    
- **컨테이너 자원 모니터링 Addon**
    
    어떤 시스템이던 가장 중요한 부분 중 하나는 바로 모니터링.
    
    Kubernetes 환경에서도 모니터링을 제공하는 Addon이 존재함.
    
    Cluster 내부에서 실행 중인 Container의 CPU, 메모리와 같은 리소스 데이터를
    
    저장하고 볼 수 있는 방법을 제공하는 Addon.
    
    **종류 :** Prometheus, Grafana, heapster, cAdvisor
    
- **Virtual Machines Addon**
    
    Kubernetes는 Container Orchestration이지만
    
    Container 영역을 벗어나서 VM 도 Kubernetes 가 관리할 있도록 해주는 Addon.
    
    **종류 :** kubeVirt
    
- **클러스터 로깅 Addon**
    
    Kubernetes Cluster 내부에서 생성되는 모든 Log를 중앙화 하여 관리할 수 있음.
    
    Master Node, Worker Node, Worker Node 내부의 Container 등 모든 로그를 중앙화 된
    
    로그 수집 시스템에 모아서 볼 수 있도록 해주는 Addon.
    
    클라우드 서비스를 이용 중이라면 각 클라우드 벤더에서 제공해주는 로깅 서비스를 사용하면 되지만
    
    직접 kubernetes를 설치한 경우에는 Log 부분도 고려해야함
    
    해당 addon 은 직접 kubernetes 를 설치한 경우에 사용하는 Addon.
    
    **종류 :** ELK
