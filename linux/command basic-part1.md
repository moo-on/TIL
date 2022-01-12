**명렁어 기본 구조**

command [option]... [argument]...

- command : 시스템에 설치되어있는 프로그램 이름(지정된 위치)
- option : command 실행 시 출력 값 조정
- argument : command 실행 시 적용 대상

**화면 넘기기**

clear or ctrl + l 

**도움말**

command —help

man command

### 명령어 모음

**기본**

**pwd** 현재 디렉토리 출력

**ls** 현재 디렉토리 파일 출력

- -l list 한 줄 출력

**whoami** 현재 계정 출력

**echo** 텍스트 등 출력

echo 1234 > fileA  - fileA에 1234 작성

**date** 날짜 출력

**history** 사용했던 명령어 목록 출력

**이동**

cd ./  현재 디렉토리에서 이동

cd  ../../  상위 디렉토리 2개

cd /  root디렉토리, 절대경로

cd ~  홈디렉토리 

**파일내용 보기 및 탐색**

**cat** F   전체내용

**head** -n 5 F  위에서 5줄 보기

**tail** -n 5 F  밑에서 5줄

**more** F  한 페이지씩 보기

**more** /root/F 경로에있는 페이지 보기

**less** F 원하는대로 본다, q를 통해 빠져나옴

**cat** F | grep keyword  원하는 키워드가 파일에 있는지 출력

- .파일명은 기본적으로 숨긴 파일이다.
- .bashrc에 alias 설정되어있음
    - rm cp mv : rm - i  cp -i mv -i 명령어 시 한번 더 물어보기

**file** name  name이라는 파일이 어떤 파일인지 알려준다.

**파일 관리 명령어**

|  | 파일 | 디렉토리 |
| --- | --- | --- |
| 생성 | touch  vim | mkdir |
| 이동 | mv name location | mv name location |
| 복사 | cp F location  | cp -r dir location |
| 삭제 | rm | rm -r |
- **cp** fileA dirA/file1  dirA에 file1이름으로 복사
- **mv** fileA fileB 이름변경 fileA →  fileB5
- -f 강제 삭제 옵션 → 물어보지않는다
- **rm** -rf dirA → dirA강제 삭제
- **rmdir** 내부파일이 없는 경우만 삭제 가능하다.

**검색**

- locate
    - 기존 데이터베이스에서 검색, 시스템 구조 변경되면 안된다.
    - 목록 데이터베이스 주기적 갱신을 통해, 빠른 색인 보장
    - 조건 입력 불가능
    - **locate** file == **find** / -name “*file*”
    - [sudo] yum -y install mlocate
- find
    - 직접 접근해서 찾기에, 시간이 오래걸린다.
    - 다양한 조건으로 검색 가능하다
    - 검색과 동시에 추가 작업이 가능하다
    - find [경로]
        - find /  기본루트부터 전 범위 탐색
        - find . 현재루트부터 범위 탐색
    - find / -name “file”
        - 정확히 file과 이름이 같을 때만 찾는다.
    - find / -name “file*”
        - file로 시작하는 파일들을 다 찾는다.
    - find / -name "file" -size +30k -size -50k
        - 30k이상 50k이하 파일
    - find / -name "*file*" -size +30k -size -50k -exec cp {} dirA \;
        - exec 옵션을 사용하여 복사, exec 사용 시 끝에 \;  붙여야된다.
        - { } 해당 괄호안에 앞에서 찾은 것들이 들어간
    - find / -name "file" -size +30k -size -50k > fileA
        - fileA에다가 찾은 것들을 목록화 하여 저장한다.
