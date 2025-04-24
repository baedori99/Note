---
title: Database
create_date: 2024-08-02 11:08 - 2024-08-02 11:08
draft: false
---
아마 DB 관련한 일이 많을 거 같기 때문에 DB에 대해서 조금이라도 배경 지식을 얻고자 정리한다.  

>[!Summary]
>- **데이터베이스(Database, DB)란?** 데이터의 저장소
>- **DBMS(Database Management System, 데이터베이스 관리 시스템)란?** 데이터베이스를 운영하고 관리하는 소프트웨어
>	- 계층형, 망형, 관계형 DBMS 중 대부분의 DBMS가 테이블로 구성된 관계형 DBMS(RDBMS)형태로 사용된다.  
>- **SQL(Structured Query Language)란?** 구조화된 질의 언어라는 뜻으로 관계형 데이터베이스에서 사용되는 언어, 표준 SQL을 배우면 대부분의 DBMS를 사용할 수 있다.  

## 데이터베이스(Database, DB)란?

데이터베이스를 한 마디로 정의하며 '**데이터의 집합'** 이라고 할 수 있다.  

데이터베이스에는 일상생활 대부분의 <U>정보가 저장되고 관리된다.</U>  
오늘 보내거나 받은 카카오톡 메시지, 인스타그램에 등록한 사진, 버스/지하철에서 찍은 교통카드, 카페에서 구매한 아이스 아메리카노 등의 정보가 모두 데이터베이스에 기록된다.  

## DBMS란?

데이터베이스를 '데이터의 집합'이라고 정의한다면, 이런 **데이터베이스를 관리하고 운영하는 소프트웨어를 DBMS(Database Management System)** 라고 한다.  

다양한 데이터가 저장되어 있는 데이터베이스는 여러 명의 사용자나 응용 프로그램과 공유하고 동시에 접근이 가능해야 한다.  

- 예시
	- 은행의 예금 계좌는 많은 사람들이 가지고 있다. 여러 명의 예금 계좌 정보를 모아 놓은 것이 데이터베이스이다.  
	- 은행이 가지고 있는 예금 계좌 데이터베이스에는 여러 명이 동시에 접근할 수 있다.  
	- 예금 계좌 주인, 은행 직원, 인터넷 뱅킹, ATM 기기 등에서 모두 접근이 가능하다. 
	- 이러한 것이 가능한 이유는 **DBMS**가 있기 때문이다.  

>[!example]- 예시
>![](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/DBMS_%EA%B0%9C%EB%85%90.png)
>

## DBMS의 종류

DBMS와 같은 소프트웨어는 특정 목적을 처리하기 위한 프로그램이다.  

- 예시
	- 문서를 작성하기 위한 소프트웨어는
		- 아래아한글(HWP)
		- 워드(Word)
	- 표 계산을 위한 소프트웨어는
		- 엑셀(Excel)
		- 캘크(Calc)
	- 사진을 편집하기 위한 소프트웨어는 
		- 포토샵(PhotoShop)
		- 김프(Gimp)

데이터베이스를 사용하기 위한 소프트웨어(DBMS)

- MySQL
- 오라클(Oracle)
- SQL 서버
- MariaDB
- Etc.

### DBMS 특징


|    DBMS    |    제작사     |           작동 운영체제           |                기타                 |
| :--------: | :--------: | :-------------------------: | :-------------------------------: |
|   MySQL    |   Oracle   | Unix, Linux, Windows<br>Mac |           오픈 소스(무료), 상용           |
|  MariaDB   |  MariaDB   |    Unix, Linux, Windows     | 오픈 소스(무료), MySQL 초기 개발자들이 독립해서 만듦 |
| PostgreSQL | PostgreSQL |  Unix, Linux, Windows, Mac  |             오픈 소스(무료)             |
|   Oracle   |   Oracle   |    Unix, Linux, Windows     |           상용 시장 점유율 1위            |
| SQL Server | Microsoft  |           Windows           |         주로 중/대형급 시장에서 사용          |
|    DB2     |    IBM     |    Unix, Linux, Windows     |          메인프레임 시장 점유율 1위          |
|   Access   | Microsoft  |           Windows           |                PC용                |
|   SQLite   |   SQLite   |        Android, iOS         |         모바일 전용, 오픈소스(무료)          |

## DBMS의 분류

DBMS의 유형은 계층형(Hierarchical),  망형(Network), 관계형(Relational), 객체지향형(Object-Oriented), 객체관계형(Object-Relational) 등으로 분류된다.  

현재 사용되는 DBMS 중에는 <U>관계형 DBMS가 가장 많은 부분을 차지하며</U>, MySQL도 관계형 DBMS에 포함된다.  

### 계층형 DBMS (Hierarchical DBMS)

- 각 계층은 트리(tree)형태를 갖는다.  
- 처음 구성을 완료한 후에 이를 변경하기가 까다롭다.  
- 다른 구성원이 찾아가는 것이 비효율적이다.  
- *지금은 사용하지 않는 형태이다.* 

![](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%EA%B3%84%EC%B8%B5%ED%98%95DBMS.png)  

### 망형 DBMS (Network DBMS) 

- 계층형 DBMS의 문제점을 개선하기 위해 등장했다.  
- 하위에 있는 구성요소끼리도 연결된 유연한 구조다.  
- 망형 DBMS를 잘 활용하려면 프로그래머가 모든 구조를 이해해야만 프로그램 작성이 가능하다는 단점이 존재한다.  
- *지금은 거의 사용하지 않는 형태이다.*

![](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%EB%A7%9D%ED%98%95DBMS.png)   

### 관계형 DBMS (Relational DBMS)

- RDBMS라고도 불린다.  
- MySQL뿐만 아니라, 대부분의 DBMS가 RDBMS 형태로 사용된다.  
- RDBMS의 데이터베이스는 테이블(Table)이라는 최소 단위로 구성된다.  
	- 이 테이블은 하나 이상의 열(column)과 행(row)으로 이루어져 있다.  
- RDBMS에서는 모든 데이터가 테이블에 저장된다.  

📌 이 구조가 가장 기본적이고 중요한 구성이기 때문에 **RDBMS는 테이블로 이루어져 있으며, 테이블은 열과 행으로 구성되어 있다는 것**을 파악했다면 RDBMS를 어느 정도 이해했다고 할 수 있다.  

![](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/sql_table.png)  


## SQL : DBMS에서 사용하는 언어

SQL(Structured Query Language)은 관계형 데이터베이스에서 사용되는 언어이다.  

관계형 DBMS 중 MySQL를 배우려면 SQL을 필수로 익혀야 한다.  



>[!reference]
>[데이터베이스 이해하기 Database(DB), DBMS, SQL의 개념](https://hongong.hanbit.co.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-databasedb-dbms-sql%EC%9D%98-%EA%B0%9C%EB%85%90/)  