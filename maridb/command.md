# mysql Command

## 접속
mysql -u 계정명 -p 

## DB 확인
show databases;
use 데이터베이스명;
show tables;
select 속성1, 속성2, ...., 속성n from 테이블명;
select * from 테이블명;

## 사용자 계정 추가
use mysql;
insert into user(host,user,password) values('localhost','계정명',password('비밀번호'));로컬접근 허용
insert into user(host,user,password) values('%','계정명',password('비밀번호'));외부접근 허용
flush privileges;

## db 생성후 db에 계정연결
grant all privileges on DB명.* to 계정명@localhost identified by '비밀번호' with grant option;
flush privileges;


## 특정 사용자의 외부접근을 허용
update user set host='%' where host='localhost' and user='계정명';
flush privileges;
