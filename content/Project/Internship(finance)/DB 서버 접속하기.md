---
title: DB 서버 접속하기
create_date: 2024-08-09 00:08 - 2024-08-09 00:08
draft: false
---
프로젝트를 하기 위해서는 DB 서버에 접속하여 DB 구성을 해야하는데 DB 구성을 하기 전 DB 서버에 접속하는 방법부터 알아보자  

팀원분이 DB 관련해서 DBeaver라는 프로그램으로 하셨기에 나도 DBeaver 프로그램으로 접속하겠다.  

보안상 문제가 될 수 있으니 블로그 사진으로 대체하겠다.  

## DB 서버 접속 방법

### 1. 데이터베이스 연결

- Dbeaver 실행 -> 데이터베이스 선택 -> 새 데이터베이스 연결 

![](https://imgur.com/k5En9aU.png)  

### 2. 데이터베이스 선택

- 연결하고자 하는 데이터베이스 선택
📌 이번 프로젝트는 MySQL로 진행하기에 MySQL로 하였다.  

![](https://imgur.com/kvaUPTV.png)  

### 3. 데이터베이스 정보 입력

- 데이터베이스 정보 입력 -> Test Connection 

	- Server Host: DB IP
	- Database: 작성하지 않아도 무관
	- Port: MySQL에 접속하므로 3306
	- Username: DB 접속 name
	- Password: DB 접속 password  

![](https://imgur.com/P33qMCC.png)  

### 4. 연결 테스트 확인

- DB 접속 성공시: 확인

	- DB 접속 성공: Connected
	- DB 접속 실패: Connection error

### 5. 데이터베이스 연결 및 완료

- Test Connection을 통해 접속 확인이 되면 완료

![](https://imgur.com/nmOh5xt.png)  

### 6. 접속하고자 하는 데이터베이스 확인

- 데이터베이스 정보를 설정하였을 때는 DB 접속이 바로 되지 않아 따로 연결 해줘야 한다.  

![](https://imgur.com/AayrmEE.png)  

### 7. 데이터베이스 연결

- 접속하고자 하는 DB 선택 -> 오른쪽 마우스 클릭 -> 연결 클릭

![](https://imgur.com/NpJ8y42.png)  

### 8. 데이터베이스 접속 및 연결

- 정상으로 데이터베이스 접속 및 연결이 되면 아래 화면처럼 접속 표시가 보인다.  

![](https://imgur.com/haFZWHc.png)  


---
>[!reference]
>[DBeaver 데이터베이스 연결](https://hotinme35.tistory.com/48)  