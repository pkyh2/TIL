# MySQL 명령어 정리

- `CREATE DATABASE db_name;` : DB생성

- `SHOW DATABASES;` : DB 조회

- `USE db_name;` : DB 선택

- `DROP DATABASE db_name;` : DB 삭제

- `/usr/bin/mysql -u root -p` : DB 접속 or `mysql`

- `drop database` 안될 때 - `systemctl restart mysql` (Mysql 나가서)

  

- `create user 'yongDB'@'%' identified by 'yong1';` : 계정생성 id, pw

- `grant all privileges on \*.\* to 'yongDB'@''