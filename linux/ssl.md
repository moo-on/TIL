### SSL

- 개인키 구축하고 인증서 만들기

```bash

# 개인키 생성
openssl genrsa -out private.key 2048  

# CSR(Certificate Signing Request)생성, 인증기관에게 전달용
openssl req -new -key private.key -out cert.csr 

# CRT 인증서 양식은 509, 실제 서비스에 사용할 인증서
openssl x509 -req -signkey private.key -in cert.csr -out cert.crt

# 작업 디렉토리를 인증서 보관 폴더로 옮기기
mv /root/cert.crt /etc/pki/tls/certs
mv /root/private.key /etc/pki/tls/private/

# SELinux 해당 디렉토리에 재설정하여서 아파치 계정이 접근할 수 있도록 권한 변경 
restorecon -Rv /etc/pki/tls/

# private key를 소유자 외에는 읽지도 못하게 만든다.
chmod 600 /etc/pki/tls/private/private.key
```

- https 서버 만들기

```bash
ymu -y install mod_ssl

# virtualHost 부분 수정해서 하나의 서버에 여러개의 호스트 만들기 가능
vim /etc/httpd/conf.d/ssl.conf
'''
 56:<VirtualHost _default_:443>
   :DocumentRoot "/var/www/html"
   :ServerName www.linux.com:443
	 :....
100:SSLCertificateFile /etc/pki/tls/certs/cert.crt
107:SSLCertificateKeyFile /etc/pki/tls/private/private.key
'''
시스템 재시작과 방화벽 허용
```
