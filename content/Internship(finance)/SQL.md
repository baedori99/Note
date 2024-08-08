---
title: SQL
create_date: 2024-08-05 17:08 - 2024-08-05 17:08
draft: false
---
이 노트는 SQL에 대해 이론으로만 조금 접해본 경험이 있어 SQL애 대한 기본 문법, 구조를 익히기 위해 적는다.   

실습은 `MySQL`로 하였다.  
공부의 출처는 [생활코딩](https://opentutorials.org/course/3161)의 영상을 보며 공부했다.  

## 1. 데이터베이스의 목적

### 스프레드시트와 데이터베이스의 차이점

-  데이터베이스는 컴퓨터 언어(SQL)를 통해 제어가 가능하다.  
- 스프레드시트는 사용자가 데이터를 클릭해서 조작한다.  
- 데이터베이스에 저장된 데이터를 웹을 통해 사람들에게 공유할 수 있다.  
- 데이터베이스에 있는 정보를 전세계에 있는 누구나 이 웹사이트에 접속해서 볼 수 있게 할 수 있다.  
- 누구나 웹사이트에 데이터를 저장할 수 있는데 그 데이터는 결국 데이터베이스에 저장되고 있다.  

| 구분  | 엑셀(스프레드시트)                                         | 데이터베이스(DBMS)                              |
| --- | -------------------------------------------------- | ----------------------------------------- |
| 장점  | 직관적, 익히기 쉽다.                                       | 사용시 쿼리 언어에 능숙해야함.<br>데이터 모델링 시 지식과 숙련도 필요 |
| 단점  | 많은 데이터 처리시 느려짐(저성능)<br>다소 복잡한 내용 청리 어려움<br>동시작업 불가 | 많은 데이터 쉽게 처리(고성능)<br>복잡한 처리 가능<br>동시작업 가능 |

---
## 2. MySQL의 구조

>[!Summary]
>- 관계형 데이터베이스는 스프레드 시트와 유사하게 표(Table) 형태로 데이터를 저장한다.  
>- 이러한 표(Table)이 모여 스키마(Schema), 스키마(Schema)가 모여 데이터베이스 서버(Database Server)가 된다.  
>- 스키마(Schema)는 다른 말로 데이터베이스(Database)라고 한다.  

`MySQL`에는 3개정도의 구성요소를 가지고 있다.  

`MySQL` 더 나아가 관계형 데이터베이스(RDBMS)는 엑셀과 같은 스프레드시트와 비슷한 구조를 가지고 있다.  

### Table(표)  
정보는 결국에는 표에 저장이 된다.  
표는 서로 연관된 데이터들을 모아 놓은 Table이다.   

![](https://imgur.com/60kP1yH.png)

### Database(데이터베이스) / Schema(스키마)

**데이터베이스**란 MySQL에서는 서로 연관된 표들을 그룹핑해서 연관되어 있지 않은 표들과 분리하는 폴더와 같은 개념이다.  

스키마는 데이터베이스와 같은 의미의 개념이다.  

![](https://imgur.com/PcG5vnp.png)

### Database Server(데이터베이스 서버)

**데이터베이스 서버**란 데이터베이스(스키마) 즉, 폴더들이 모여 있는 큰 폴더이다.   

MySQL을 설치했다는 것은 데이터베이스 서버라는 프로그램을 설치했다는 뜻이다.  그 프로그램이 가지고 있는 기능성을 이용해 데이터와 관련된 여러가지 작업을 하는 것이다.  

![](https://imgur.com/2xrEZB0.png)

---
## 3. 서버 접속

- 데이터베이스를 통해 얻을 수 있는 효용
	1. 보안 - 데이터베이스 자체적인 보안체계를 가지고 있어 안전하게 데이터를 보관할 수 있다.  
	2. 권한 기능 - MySQL에 여러 사람 등록이 가능하다.  
		- 차등적으로 권한을 줄 수 있는 기능이 있다.  

MySQL 서버에 접속하기 위한 방법으로는  

1. 일단 `C:\Program Files\MySQL\MySQL Server 8.0\bin` 디렉토리에 들어가 있어야한다.  

![](https://imgur.com/6VkXPbo.png)  

2. `mysql -u root -p`

- `-u`: User(유저)의 약자이다.  
- `root`: `root`라는 사용자로 접속하겠다는 의미이다.  
	- 다른 사용자로 바꿀 수 있다.  
	- `root`는 관리자이기 때문에 모든 권한이 열려 있다.  
	- **`root`의 권한으로 데이터베이스를 직접 다루는 것은 위험하다.**
	- 중요한 시스템이라면 별도의 사용자를 만들어서 작업을 하다 중요한 일이 있을 때만 `root`로 들어가는 것이 권장된다.  
- `-p`: `-p`뒤에 비밀번호를 칠 수 있지만 비밀번호가 보이기엔 `-p`까지만 쓴다.
	- 이후 MySQL에서 비밀번호를 물어본다.  

![](https://imgur.com/4XM6cFG.png)  

비밀번호를 치고 나면

![](https://imgur.com/sd78hPP.png)  

MySQL 서버에 접속하게 된다.  

지금 이 상태는 위의 [[SQL#Database Server(데이터베이스 서버)|데이터베이스 서버]] 에 있는 그림을 보면 데이터베이스 서버 안으로 들어온 상태라고 볼 수 있다.  

---
## 4. Schema(스키마)의 사용  

1. 스키마(데이터베이스)를 생성하는 방법

`CREATE DATABASE 데이터베이스이름;`  

- 예시
```sql
CREATE DATABASE opentutorials;
```

>[!reference]-
>[MySQL 공식문서 Creating and Selecting a Database](https://dev.mysql.com/doc/refman/8.4/en/creating-database.html)  

2. 스키마(데이터베이스)를 삭제하는 방법

`DROP DATABASE 데이터베이스 이름;`  

- 예시
```SQL
DROP DATABASE opentutorials;
```

📌 인생에서 `DROP DATABASE`를 할 일은 거의 없다.  

>[!reference]- 
>[구글검색: mysql drop database](https://www.google.com/search?q=mysql+drop+database&oq=mysql+drop+database&gs_lcrp=EgZjaHJvbWUyCQgAEEUYORiABDIHCAEQABiABDIMCAIQABgUGIcCGIAEMgcIAxAAGIAEMgcIBBAAGIAEMgcIBRAAGIAEMgcIBhAAGIAEMgcIBxAAGIAEMgcICBAAGIAEMgcICRAAGIAE0gEIMzkxMmowajeoAgiwAgE&sourceid=chrome&ie=UTF-8)  

3. 스키마(데이터베이스)리스트를 보는 방법  

`SHOW DATABASES;`  

- 예시
```sql
SHOW DATABASES;
```

>[!example]- 실행 결과
>![](https://imgur.com/3IlRk0G.png)  

>[!reference]-
>[구글 검색: how to show database list in mysql](https://www.google.com/search?q=how+to+show+database+list+in+mysql&oq=how+to+show+database+list+in+mysql&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIKCAEQABiABBiiBDIKCAIQABiABBiiBDIKCAMQABiABBiiBNIBCTExNjkzajBqN6gCCLACAQ&sourceid=chrome&ie=UTF-8)  

4. 테이블을 만들기 전 스키마(데이터베이스)를 사용하는 방법

`USE 데이터베이스이름;`  

- 예시
```sql
USE opentutorials;
```

>[!example]- 실행 결과
>![](https://imgur.com/0DTRU2o.png)  

위의 코드의 뜻은 내가 내리는 명령을 `opentutorials`라고 하는 스키마(데이터베이스)에 있는 표를 대상으로 명령을 실행한다는 의미이다.  
- 다른 말로 "이 명령어 이후의 쿼리는 해당 데이터베이스에 적용된다."라는 의미이다.  

---
## 5. SQL과 테이블 구조

SQL은 Structured Query Language의 약자이다.  

### **SQL**
- **S**tructured: 관계형 데이터베이스가 표의 형태로 정보가 구조화되어 있어서 Structured라는 표현이 쓰였다.  
- **Q**uery: 데이터 관련 명령(생성, 읽기, 수정, 삭제 등)들을 포괄적으로 데이터베이스에게 요청(질의)한다라는 뜻에서 쓰였다.  
- **L**anguage: 데이터베이스와 사용자가 서로 이해할 수 있는 공통의 약속에 따라서 사용하는 언어가 SQL이라는 언어다.  

📌SQL 컴퓨터 언어의 두가지 특징

1. 쉽다.  
2. 중요하다.  
	- SQL이라는 컴퓨터 언어는 관계형 데이터베이스라는 카테고리에 속하는 제품들이 공통적으로 데이터베이스 서버를 제어할 때 사용하는 표준화된 언어다.  
	- 압도적인 다수의 데이터베이스 시스템이 SQL을 통해서 동작하고 있다.  

### Table의 구조

![](https://imgur.com/cRzLRxG.png)  

- Column(열): 데이터베이스에서 데이터의 타입/데이터의 구조라고 생각하면 된다. 
- Row(행): 데이터 하나하나 데이터 자체를 말한다.  

---
## 6. MySQL 테이블의 생성

📌 스프레드 시트와 데이터베이스의 차이는 데이터베이스의 아주 중요한 기능인 Column에 Data Type을 강제할 수 있다는 것이다.  

- 스프레드 시트로 만든 Table
![](https://imgur.com/owkPqqM.png)

- 위의 Table을 만들 SQL Query문
![](https://imgur.com/4LYY9Iq.png)  

- Table을 생성하는 방법 검색하기

구글에 `create table in mysql cheat sheet`를 쳐서 테이블 생성에 대한 정보를 얻는다.  

![](https://imgur.com/J2c7PJQ.png)  

>[!reference]-
>[SQL Cheat Sheet](https://www.sqltutorial.org/sql-cheat-sheet/)  
>- 블로그에 더 좋은 자료가 있기에 넣어둔다.  

### Table 생성

위의 스프레드 시트의 테이블 `topic`을 MySQL을 통해 만들어 볼 것이다.  
1. `id` column 만들기

```sql
id INT(11) NOT NULL, AUTO_INCREMENT
```

- `INT(N)`: `datatype`으로 괄호안의 숫자는 나중에 검색할 때 얼마까지만 노출시킬 것인지를 정하는 것이다.  
- `NOT NULL`: 값이 없는 것을 허용하지 않겠다.(= 무조건 값이 있어야 한다.)
- `AUTO_INCREMENT`: 자동으로 1씩 증가한다.  
	- 중복되지 않는 식별자를 갖기 위함이다.  

>[!reference]-
>[MySQL Datatype 정리](https://www.techonthenet.com/mysql/datatypes.php)  

2. `title` column 만들기  

```sql
title VARCHAR(100) NOT NULL
```

- `VARCHAR`: 사이즈가 변할 수 있는 string을 말한다.  
	- `VAR`: Variable의 약자
	- `CHAR`: Character의 약자

3. `description` column 만들기

```sql
description TEXT NULL
```

- `TEXT`: `VARCHAR`보다 긴 문자열을 저장할 수 있는 datatype
- `NULL`: 값이 없다.  

4. `created` column 만들기

```sql
created DATETIME NOT NULL
```

- `DATETIME`: 날짜와 시간 모두를 표현할 수 있는 datatype

5. `author` column 만들기  

```sql
author VARCHAR(30) NULL
```

6. `profile` column 만들기

```sql
profile VARCHAR(100) NULL
```

7.  `PRIMARY KEY` 만든다. 

```sql
PRIMARY KEY(id)
```

- `PRIMARY KEY`: row을 식별할 때 기준이 되는 키
	- `id`의 값이 중복이 일어나게 되면 식별자로서의 역할을 수행하기 어렵다.  
	- 이를 막기 위해 `id`를 `PRIMARY KEY`로 설정하여 테이블에서 각 행을 식별하는 고유한 값을 만들 때 사용한다.  

- PRIMARY KEY 특징
	- 각 row에 대해 고유하다.(Unique)  
	- `NULL` 값이 허용 안된다. (`NOT NULL`= <U>무조건 값이 있어야 한다</U>.)
	- row를 식별하는 데 사용되고 <U>테이블당 하나의 기본키만 지정 가능</U>하다.   

**모든 코드를 합쳐 테이블을 만든다면**  

```sql
CREATE TABLE topic (
	id INT(11) NOT NULL AUTO_INCREMENT,
	title VARCHAR(100) NOT NULL,
	description TEXT NULL,
	created DATETIME NOT NULL,
	author VARCHAR(30) NULL,
	profile VARCHAR(100) NULL,
	PRIMARY KEY(id)
);
```

>[!example]- 실행 결과
>![](https://imgur.com/rahVvJr.png)  

>[!error]- ERROR 1820(HY000) 에러
>![](https://imgur.com/RZTJAUo.png)  
>
>- 해결 방법
>![](https://imgur.com/gIYYz8Q.png)
>`SET PASSWORD = PASSWORD('your_new_password')`

---
>[!reference]
>[생활코딩 DATABASE2 - MySQL 유튜브 재생목록](https://www.youtube.com/playlist?list=PLuHgQVnccGMCgrP_9HL3dAcvdt8qOZxjW)  
>[생활코딩 MySQL의 구조](https://daco2020.tistory.com/24)  
>[생활코딩 - MySQL - 5. 서버접속](https://act-think.tistory.com/135)  
>[MySQL- MySQL의 구조, 서버 접속, 스키마의 사용](https://sunandbean.tistory.com/319)  
>[SQL-SQL과 테이블 구조(생활코딩)](https://khoonstory.tistory.com/m/33)  
>[생활코딩- MySQL - 8. 테이블의 생성](https://act-think.tistory.com/138)  
>[SQL/MySQL/기본키, 고유키, 외래키](https://boring9.tistory.com/54)  


