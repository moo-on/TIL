**vagrant 기본 커맨드**

```bash

vagrant up
vagrant halt
vagrant destroy

vagrant plugin install vagrant-hostmanager
vagrant plugin install vagrant-disksize

--각 머신에서 하는 작업--
sudo -i
vim /etc/ssh/sshd_config
PasswordAuthentication No → PasswordAuthentication Yes
systemctl restart ssh.service

--생성된 머신에 접속--
vagrant ssh kube-control1
ssh vagrant@vm_ip_addr
```

**kubeadm을 이용한 모든 머신에 쿠버네티스 설치**

```bash

# 모든 vm에 설치 #
# Ubuntu APT Repository 추가를 위한 패키지 설치
sudo apt-get update -y
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release -y

# Docker Repository 지정패스에 추가
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update -y

# Docker Engine 및 containerd.io Container Runtime 설치
sudo apt-get install docker-ce docker-ce-cli containerd.io -y

# 구글 클라우드 공개 사이닝 키 다운로드 지정 패스에 저장
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

# 쿠버네티스 apt레포 추가
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

# apt 패키지 색인을 업데이트하고, kubelet, kubeadm, kubectl을 설치하고 해당 버전을 고정한다.
sudo apt-get update
sudo apt-get install -y kubelet=1.19.11-00 kubeadm=1.19.11-00 kubectl=1.19.11-00
sudo apt-mark hold kubelet kubeadm kubectl

```

**kube-control1에서 작업 — Control Plane**

```bash
# pod 네트워크에서 사용하는 192.168.0.0/16 네트워크 대역대를 설정 # 오래걸림 주의
sudo kubeadm init --control-plane-endpoint 192.168.56.11 --pod-network-cidr 192.168.0.0/16 --apiserver-advertise-address 192.168.56.11

# k8s 클러스터에 접근할 수 있는 사용자 설정하기
# k8s 설정 디렉토리 생성
mkdir -p $HOME/.kube

#kubectl 권한 파일 추가 및 유저에게 권한 설정
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# calico 라는 network namespace 역할 해주는 컨트롤러 생성
kubectl create -f https://docs.projectcalico.org/manifests/calico.yaml

# 결과 값 복사 
kubeadm join 192.168.56.11:6443 --token 0zz6qz.ydloxad8uvbh5htp \
    --discovery-token-ca-cert-hash sha256:0d07cd0ca2235a00d941c7e076b59a6422f13d7c27fc107c41c8d8cb48f18a79

---각 노드들 연결 작업 후---
kubectl get nodes # 연결 노드 상태
kubectl cluster-info # 클러스터 정보
```

**각 노드에 컨트롤 플레인에 연결되는 K8s 사용자 계정을 생성한다.**

```bash
# 결과 값에 sudo 붙여서 복사해놓기 kubeadm join ...을 다른 워커 노드에 복사해서 실행하기

sudo kubeadm join 192.168.56.11:6443 --token ***TOKEN_VALUE*** \
--discovery-token-ca-cert-hash sha256:***HASH_VARLUE***
```
