# AWS Storage Guide - EBS, EFS

**storage종류**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e30f6e20-2dd1-439a-81ae-ea07562b2a00/Untitled.png)

**S3**

- **버킷**은 s3의 최상위 레벨로, 모든 리전에서 유일해야한다
    - 계정별 100개의 버킷 생성 가능
    - ACL 역할 수행
- **객체**는 버킷에 저장한 모든 데이터이다.
    - 데이터와 메타데이터(수정일 파일타입 ...)를 갖는다.
    - 버전관리가 가능하다
- **접근방법**은 Rest API/ SDK/ HTTP-method/ AWS-CLI

```bash
-----aws-CLI----
s3 bucket 생성 -> 접근 가능 IAM 사용자(administer) 생성 및 사용자 key.csv저장 
-> CLI 다운 -> cmd $ aws configure -> 연결 완료 -> s3 push pull 가능
 
```

**EC2 → 추가 EBS 설정** 

- **EBS란?**
    - 블록 스토리지
    - 단일 인스턴스에만 연결 가능
    - EC2의 기본 EBS는 종료되면 사라진다. → 휘발성
    - EBS 따로 생성 시 → 비휘발성

```bash
fdisk /dev/xvdf
partprobe

mkfs.ext4 /dev/svdf1
mkdir /home/ec2-user/backup/

mount /dev/xvdf1 /home/ec2-user/backup/
umount /home/ec2-user/backup/

file -s /dev/xvdf
lsblk
df -TH

----영구마운트 파일 설정----
vim /etc/fstab

```

**EC2 → EFS 추가**

- **EFS란?**
    - NFS 기반 공유 파일 시스템

```bash
1. 보안그룹에서 EFS가 접속할 수 있도록 접속할 instance 접근 추가 설정 
	ex) 인스턴스에는 상관없고, EFS의 보안그룹에 NFS 허용만 설정하면 된다.
2. EFS-Network NFS가 허용된 보안 그룹 설정
3. EFS 연결 - guide 에 따라 시스템에서 불러올 폴더를 선택해서 연결한다.
```

주의 : 모든 스토리지 분리 시 시스템 내부에서 ‘umount {directory_path}’
