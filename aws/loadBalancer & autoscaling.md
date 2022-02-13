# 로드밸런서 & 오토스케일링

### 로드밸런서

**로드 밸런서 정의**

애플리케이션 및 네트워크 트래픽을 자동 분산한다.

- **실습) 로드 밸런서 접속하면, 그룹화된 인스턴스가 번갈아가면서 요청 받기**

```bash
----첫번째 인스턴스 -> blog ---
1. AZ-a에 인스턴스 생성 
2. http포트 열기
3. sudo echo "hello" > /var/www/html/index.html

----두번째 인스턴스 -> news ---
1. AZ-b에 인스턴스 생성 
2. http포트 열기
3. sudo echo "load_balancer" > /var/www/html/index.html

----ELB 생성----
1. ELB(ALB) 생성
2. 로드밸런서 대상 그룹에 web-server로 인스턴스 blog, news 추가
3. 가용영역 a,b 추가
4. 리스너 포트 80
5. 대상 그룹 web-server를 해당 로드밸런서에 추가
6. 로드밸런서에 접속하면 blog, news instance가 번갈아가면서 요청을 수락한다.

#!/bin/bash
sudo yum update
sudo yum -y install httpd
sudo systemctl restart httpd
sudo systemctl enable httpd
sudo echo "hello" > /var/www/html/index.html
```

- **실습) 로드 밸런서 접속하면, 특정 주소는 특정 인스턴스가 요청 받기**

```bash
----첫번째 인스턴스 -> blog ---
1. sudo echo "hello blog" > /var/www/html/blog/index.html

----두번째 인스턴스 -> news ---
1. sudo echo "hello news" > /var/www/html/news/index.html

----ELB 편집----
1. 로드밸런서 대상 그룹에 server-blog, server-news로 인스턴스 blog, news 추가
2. 리스너 포트 80 규칙 편집
3. ELB_address/blog로 접속 시 blog 인스턴스로만 접속, news도 같은 알고리즘
4. 로드밸런서에 /blog, /news 접속하면, 지정된 인스턴스 그룹으로 요청  
```

- **실습) 로드 밸런서 접속하면, 세션 유지 시키기**

```bash
target group 편집 속성에서 stickiness 체크 

한번 접속된 인스턴스에서 다른 인스턴스로 넘어가지 않는다.
```

 

**Amazon Auto Scaling**

서비스 구축 단계에서 최대치의 인프라를 구축할 필요가 없이,

사용자가 정의한 조건에 따라 EC2 용량이 자동으로 확장/축소

로드밸런서 그룹에 정의 해놓아야 자동으로 할당된다.
