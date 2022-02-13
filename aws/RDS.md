RDS → private subnet

**private subnet에 DB에 설치하고 webserver 연결하기**

- **DB설정**
    - 퍼블릭엑세스는 불가능
    - private subnet을 그룹화 하여 DB에 연결하기
        - 가용영역은 private subnet이 있는 곳으로 연결(private 유지)
    - 보안 인바운드 정책에서 mysql 포트 열어주기
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1f18feff-e144-4a91-abd0-d42cc02e38dc/Untitled.png)
    
- **public subnet에 webserver용 ec2 시작**
    - 보안 인바운드 정책에서 http 방화벽 해제
    
    ```bash
    ---ec2----
    
    yum update
    yum install httpd
    amazon-linux-extras install -y php7.2 lamp-maraiadb10.2-php7.2
    systemctl restart httpd
    
    mysql -u <username> -p -h <end_point>
    
    create database users;
    use users;
    
    # 테이블 설치
    CREATE TABLE exam (
    id int NOT NULL AUTO_INCREMENT,
    name varchar(30),
    email varchar(40),
    num varchar(30),
    PRIMARY KEY(id)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
    
    insert into exam(name, email, num) values("<name>", "<email>", "<num>")
    select * from exam;
    
    vim /var/www/html/index.htm
    
    index.html
    <!DOCTYPE html> 
    <html> 
        <head> 
            <meta charset="utf-8" /> 
            <title>예제 회원가입</title> 
        </head> 
    
        <body> 
            <form method="post" action="./signup.php"> 
                <label>이름 <input type="text" name='name'> </label><br> 
                <label>이메일 <input type="email" name='email'> </label><br>
                <label>전화번호 <input type="number" name='num'> </label><br> 
                <input type="submit" value='가입하기'> 
            </form> 
        </body> 
    </html>
    
    signup.php
    <?php
    $name=$_POST['name'];
    $email=$_POST['email'];
    $num=$_POST['num'];
    
    $mysql_hostname = 'endpoint';
    $mysql_username = 'admin';
    $mysql_password = 'admin';
    $mysql_database = 'users';
    $con = mysqli_connect($mysql_hostname, $mysql_username, $mysql_password, $mysql_database) or die ("Can't access DB");
    $query = "insert into exam (name,email,num) values('".$name."','".$email."','".$num."')";
    $resut=mysqli_query($con,$query);
    
    if (!$result) {
                ?> <script> alert("회원 가입 완료");location.href=".."; </script> <?php }
    else {
            ?> <script> alert('회원가입 실패했습니다.\n다시 시도하세요');location.href=".."; </script> <?php }
    ?>
    ```
