**리눅스 부팅 과정**

하드웨어 → 부트로더(커널준비) → 커널 → 파일시스템 마운트 → 드라이버 초기화 → 사용자 단계에서  systemd 실행 

**커널단계**

1. 부팅이 가능한 파티션이 있는지, 커널이 있는지 확인 → POST(Power On Self Test)를 실행
2. 시스템 펌웨어를 업로드하여 초기화 작업을 실행 → BIOS UEFI
3. 부트로더 실행
4. /boot/grub2/grub.cfg 파일에서 해당 구성을 로드하고 부팅할 커널을 선택
5. 커널은 선택하거나 제한 시간이 만료되면 부트로더가 디스크에서 커널 및 initramfs를 로드하여 메모리에 적재
    1. initramfs : 부팅, 초기화 스크립트 등에 필요한 모든 하드웨어에 대한 커널 모듈이 포함된 아카이브
6. 커널 이후 부터 주로 사용되는 파일시스템 마운트
7. initramfs에서 드라이브를 적재
8. systemd 프로세스가 initrd,target 대상에 대한 모든 유닛을 실행, 루트 파일 시스템을 디스크의 /sysroot 디렉토리에 마운트(/etc/fstab) 
    1. sysroot: root시스템 점검을 하기 위한 읽기 전용 디렉토리
9. 커널을 initramfs의 루트 파일 시스템을 /sysroot의 루트파일 시스템으로 전환함
    1. initramf /sysroot → 최적의 시스템의 /sysroot로 찾아감
10. systemd는 시스템에 구성된 기본 대상을 찾은 다음 해당 대상에 대한 구성을 준수할 유닛을 시작
- 타겟 설정 파일 /etc/systemd/system/default.target
    - allow = yes 일 시, systemctl isolate 명령으로 타겟을 바꿀 수 있음. 일시적 적용
    - systemctl isolate graphical.target
    - systemctl isolate multi-user.target
    - ls -l /usr/lib/systemd/system | grep runlevel* 에서 가능한 타겟 확인

**기본타겟확인 및 영구 수정**

systemctl get-default

systemctl set-default graphical.target

**종속성 확인**

systemctl list-dependencies graphical.target | grep target

**/etc/fstab 설정 파일 잘못되어 부팅에 문제 생겼을 경우**

1. 부트 로더 메뉴 진입
    1. e를 눌러 edit mode 활성화
2. 원하는 방법으로 진입 가능
    1. linux로 시작하는 행의 끝에서 systemd.unit=emergency.target 으로 진입 ctrl + x
3. 루트파일시스템 읽기/쓰기로 마운트 → mount -o remount,rw
4. /etc/fstab 파일에서 잘못된 줄 삭제 혹은 수정하기
5. mount -a  재마운트
6. ctrl + D 후 재부팅하기

**root비밀번호 분실했을 때**

1. 부팅 editmode에서 rd.break 입력 후 실행
2. ls 결과 값에 sysroot가 적용되기전이므로 ls에 존재 → sysroot파일 시스템이 디렉토리에 마운트 되지 않았음.
3. sysroot를 읽기/쓰기로 다시 마운트
    1. mount -o remount,rw /sysroot  
4. 기존 root를 한 단계 낮은 sysroot로 바꿔줌으로서 격리해 안정성을 높인다.
    1. chroot /sysroot
    2. 기존에 root에서 작업을 하면 기본 루트 bin과 연결되어서 작업된다.
5. 비밀번호 변경
    1. passwd
6. 저장
    1. touch /.autorelabel
    2. 변경된 사항을 파일로 만들어 적용
7. exit로 나간 후 ctrl + d 두 번 입력후 나가기
