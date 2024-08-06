---
title: Web Server Structure
create_date: 2024-08-06 23:08 - 2024-08-06 23:08
draft: true
---
웹 서버 구조에 대해서는 2024-08-06에 한 프로젝트 시작에 대해서 신규 호스팅과 서버 구도 확정에 대해 팀원과 얘기하다가 모르는 부분에 대한 정리를 하고자 만들었다.  

>[!abstract]
>1. Static vs Dynamic 페이지  
>	1. Static Pages  
>	2. Dynamic Pages  
>2. Web Server와 WAS의 차이  
>	1. Web Server  
>	2. 웹 컨테이너(Container)  
>		1. 웹 컨테이너의 작동  
>	3. WAS(Web Application Server)  
>	4. Web Server가 필요한 이유  
>	5. WAS가 필요한 이유  
>	6. Web Server + WAS 조합  

# Static vs Dynamic pages

## Static Pages (정적 웹 페이지)

데이터베이스에서 정보를 가져오는 등 별도의 서버에서의 처리가 없어도, 사용자들에게 보여줄 수 있는 페이지. 어떠한 사용자가 오던 간에 동일한 페이지를 보여준다.  

- 저장된 그대로 사용자에게 전달되는 웹페이지이다.  
- 서버에 저장된 데이터가 변경되지 않는 한 사용자는 고정된 웹 페이지를 보게 된다.  
- 정적 웹 페이지들은 업데이트를 전혀 하지 않거나 거의 할 필요가 없는 내용에 적절하다.  
- 저장된 데이터만 보여줄 수 있어 서비스가 한정적이다.  

>[!example]- 
>`image`, `html`, `css`, `javascript` 파일과 같이 컴퓨터에 저장되어 있는 파일들  

## Dynamic Pages (동적 웹 페이지)