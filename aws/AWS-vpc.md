# VPC

### VPC

**vpc정의**

- 직접 정의 가능한 가상 네트워크에서 aws리소스를 구동할 수 있는 논리적으로 격리된 네트워크

**vpc 주요 설정 값**

- IP주소 범위
- 서브넷
- 라우팅 테이블
- 네트워크 게이트웨이
- IPv4 및 IPv6

**vpc구성**

- web application  단위로 VPC 구성이 주를 이룬다.

**싱글vpc구성**

- 자격증명 간소화
- 소규모

**멀티vpc구성**

- 기능을 분리할 수 있는 application
- 다양한 환경 관리
- 자격증명 복잡

**더 알아보기**

- Subnet은 AZ 단위로 생성
- 하나의 VPC에 세개의 AZ가 있다면?
    - AZ1 전용 subnet1 AZ2 전용 subnet2 ....
- vpc는 region 단위로 생성 가능하다. vpn과 비슷한 개념.

**IG(Internet Gateway)**

- vpc와 인터넷 간의 논리적 연결
- 퍼블릭 서브넷  내의 자원이다.
- 인바운드 아웃바운드 자유
- vpc당 하나 생성

**NAT Gateway**

- 기능
    - 외부가 들어오지는 못하지만, 외부로 나갈수 있게끔 한다.
    - 프라이빗 공간에서 공인IP를 가지고 나갔다가 다시 사설 주소로 바뀌어서 들어온다.
- 사용목적
    - 프라이빗 서브넷 내에 있는 인스턴스를 인터넷 혹은 다른 AWS 서비스에 연결하기 위한 용도
- 사용조건
    - NAT게이트웨이가 생성될 퍼블릭 서브넷 지정
    - NAT게이트웨이와 연결할 EIP 필요
    - NAT게이트웨어와 통신이 가능하도록 프라이빗 서브넷과 연결된 라우팅 테이블 설정 수정
- EX)
    - ec2는 private subnet, public subnet에 있는 NAT Gateway를 통해 나간다.
    - Nat instance가 Nata Gateway역할 가능하다.
    - bastion host는 ec2로 접근만 가능하고, nat instance가 해당 기능이 포함되어 있다.
- 퍼블릭 서브넷  내의 자원이다

**실습1**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9f34d228-f4da-4587-b66b-1642adc36ea2/Untitled.png)

- VPC 생성
    - 이름: cloud_vpc , CIDR: 10.0.0.0/16
    - 라우팅 테이블 확인(default)
    - 10.0.0.0/16 → target
- 인터넷 게이트 웨이 생성
    - 이름: cloud-vpc-IGW
    - cloud-vpc에 연결
- 퍼블릭 서브넷 생성
    - vpc → 서브넷 생성
    - 이름 : cloud-vpc-public-subnet, CIDR: 10.0.1.0/24
    - 서브넷 자동 주소 활성화(30분 날림ㅠ)
- 퍼블릭 라우팅 테이블 생성
    - 라우팅 테이블 생성
    - 이름 : cloud-vpc-public-rt
    - 경로 추가 : 0.0.0.0/0 → cloud_vpc_IGW
    - 서브넷 연결
- 인스턴스 생성 후 확인
- 프라이빗 서브넷 생성
    - 라우팅 테이블 생성 → 서브넷 연결
    - NAT 게이트웨이 추가( EIP 발급 ),cloud_vpc_nat_gt
- private EC2 생성 →
    - 생성 후 Public의 EC2로 접속 후 ping 8.8.8.8
- root 권한으로 ssh접속 밑 코드 참조
- Private의 EC2가 외부와 통신되는지 확인
    
    ```python
    ```````````````````````````````
    #!/bin/bash
    (
    echo "1234"
    echo "1234"
    ) | passwd --stdin root
    sed -i "s/^PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
    sed -i "s/^#PermitRootLogin yes/PermitRootLogin yes/g" /etc/ssh/sshd_config
    service sshd restart
    ```````````````````````````````
    ```
    

**실습2- NAT Instance로 private subnet network 활성화하기**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2b0487b-816c-4232-85a7-7e5c38d9afab/Untitled.png)

- **subnet 1 - public**
    - 외부 통신 : internet gateway
    - 보유 인스턴스 : nat gateway, nat instance
        - nat instance 설치 : AMI(amzn-ami-vpc-nat) -  bastinct기능
            - 보안그룹 인바운드
                - ssh, icmp
            - 소스/대상 확인 비활성화
- **subnet 2 - private**
    - 외부 통신 : nat gateway
- **subnet 3 - private**
    - 외부 통신 : nat instance
        - subnet3가 사용할 라우팅 테이블 생성
            - 0.0.0.0/0 → nat instance의 <ENI ID> 값
    - 보유 인스턴스 : ec2-private2 - p 1234
