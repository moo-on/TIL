- local host → guest로 파일 옮기기

```bash
scp .\filename root@ip:/root/
2.4
2.7
```

- rpm파일로 설치 및 사용환경 세팅하기

```bash
yum -y remove mariadb-libs

yum -y localinstall Mari*

systemctl restart mysql
systemctl status mysql

chkconfig mysql on # 재부팅해도 계속 켜져있음

firewall-cmd --permanent --add-service=mysql # 외부에서 접속하기 위함
firewall-cmd --reload

----yum으로 대체 가능-----
yum install -y mariadb-server mariadb

systemctl restart mariadb
systemctl enable mariadb

firewall-cmd --permanent --add-service=mysql
firewall-cmd --reload

mysqladmin -u root password '1234'  # 초기비밀번호 설정

mysql_secure_installation  # 초기세팅 명령어
```

- DB 접속하기

```sql
mysql -u root -p # 비밀번호 설정 안되었을 경우, 1234

show dabases;  # 데이터베이스 확인

CREATE DATABASE member_db;  # 새로운 DB 생성

USE member_db;  # DB선택

-------TABLE----------

SHOW tables;  # 테이블 확인

EXPLAIN[DESCRIBE, DESC] table_name  # 테이블 구조 확인

CREATE TABLE member(id VARCHAR(12) NOT NULL PRIMARY KEY ...);  # 생성

DROP TABLE table_name;  # 삭제

ALTER TABLE table_name [OPTIONS];  # 수정, 컬럼 추가 변경 등

-------RECORD--------
INSERT INTO member VALUES ('kim','김철수','33','서울');  # 삽입, 순서 중요
UPDATE table_name set id='kim123' WHERE id = 'kim'# 수정, id kim을 kim123으로 수정
DELETE FROM table_name WHERE condition  # 삭제, conditionEX) id='kim'

SELECT * FROM table_name WHERE condition # 조회s
	

```

- 원격 접속

```sql
----server----
use mysql

GRANT ALL PRIVILEGES ON *.* TO test@'10.0.2.%' IDENTIFIED BY '1234';  # 권한

select user,host  from user where user NOT LIKE '';   # 확인
-----client-------
mysql -h 10.0.2.4 -u test -p  # test계정으로 접속
```
