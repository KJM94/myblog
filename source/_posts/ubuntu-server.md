---
title: ubuntu server
date: 2024-01-26 12:18:55
tags: Linux
categories:
    - Linux
---
# 우분투 mysql 연동

![](/image/캡처3.PNG)

- 우분투 서버 업데이트
```commandline
$ sudo apt-get update
```

- mysql-server 설치
```commandline
$ sudo apt-get install mysql-server
```

설치 중간에 Password를 물어보는데 일단 빈칸으로 두고 OK를 누르면<br>
Ubuntu 서버 비밀번호와 자동으로 동기화 된다.

- MySQL 기본 설정
    - 외부 접속 기능 설정 (포트 3306 오픈)<br>
  처음에 이 부분을 설정 안했더니 MySQL Workbench를 사용할 때 오류가 발생했음

```commandline
$ sudo ufw allow mysql
```

- MySQL 실행
```commandline
$ sudo systemctl start mysql
```

- Ubuntu 서버 재시작시 MySQL 자동 재시작
 ```commandline
$ sudo systemctl enable mysql
```

- MySQL 접속
```commandline
$ sudo /usr/bin/mysql -u root -p
```

ERROR 1698(28000)<br>
처음에 $ sudo mysql -u root -p 로 진행했더니 아래와 같은 에러가 발생했다.<br>

ERROR 1698 (28000): Access denied for user 'root'@'localhost' <br>

해결방법 : mysql 대신 /usr/bin/mysql 과 같이 경로를 정확하게 명시
<br>

mysql> 여기다가 입력하면 된다

- 사용자 등록 및 권한 설정
    - 사용자 정보 확인
    <br> mysql> SELECT User, Host, authentication_string FROM mysql.user;


- TESTDB 라는 데이터 베이스를 만들고 확인

mysql> CREATE DATABASE TESTDB;<br>
mysql> SHOW DATABASES;<br>
mysql> show table status;<br>

limit 5;

- TESTDB 데이터베이스를 사용할 계정 testuser 만들고 확인

mysql> CREATE USER 'testuser'@'localhost' IDENTIFIED BY 'mysql비번';<br>
mysql> FLUSH PRIVILEGES;<br>
mysql> SELECT User, Host, authentication_string FROM mysql.user;<br>

- TESTDB 데이터베이스를 사용할 계정 testuser 관한 부여

mysql> GRANT ALL PRIVILEGES ON 데이터베이스이름.* FOR'testuser'@'localhost';<br>
mysql> FLUSH PRIVILEGES;<br>
mysql> SHOW GRANTS FOR'testuser'@'localhost';<br>
mysql> SELECT User, Host, authentication_string FROM mysql.user;<br>

- Option 1. MySQL 버전 확인

mysql> show variables like "%%version";

Option 2. Mysql 비밀번호 변경 방법

mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('바꿀비번');
또는

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '바꿀비번';



