---
title: Web Server Structure
create_date: 2024-08-06 23:08 - 2024-08-06 23:08
draft: false
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

![](https://imgur.com/rhHEJt4.png)  

![](https://imgur.com/TcRzZYP.png) 


## Static Pages (정적 웹 페이지)

데이터베이스에서 정보를 가져오는 등 별도의 서버에서의 처리가 없어도, 사용자들에게 보여줄 수 있는 페이지. 어떠한 사용자가 오던 간에 동일한 페이지를 보여준다.  

- 저장된 그대로 사용자에게 전달되는 웹페이지이다.  
- 서버에 저장된 데이터가 변경되지 않는 한 사용자는 고정된 웹 페이지를 보게 된다.  
- 정적 웹 페이지들은 업데이트를 전혀 하지 않거나 거의 할 필요가 없는 내용에 적절하다.  
- 저장된 데이터만 보여줄 수 있어 서비스가 한정적이다.  

>[!example]- 
>`image`, `html`, `css`, `javascript` 파일과 같이 컴퓨터에 저장되어 있는 파일들  

## Dynamic Pages (동적 웹 페이지)

서버에서 데이터베이스에서 정보를 가져와서 처리하는 것철머, 어떠한 요청에 의하여 서버가 일을 수행하고 해당 결과가 포함된 파일을 보여주는 페이지이다. 사용자들마다 다른 페이지가 보여질 수 있다.  

- 서버가 사용자의 요청에 대하여 데이터를 가공한 후 생성되는 웹 페이지이다.  
- 사용자의 상황, 시간, 요청 등에 따라 달라지는 웹 페이지를 보게 된다. 
- 같은 페이지라도 사용자마다 다른 결과의 웹 페이지를 볼 수 있다.  
- 웹 사이트의 구조에 따라 삽입/수정/삭제 등의 작업이 용이하다.  
- 정적 페이지에 비해 속도가 느리다.  

## Web Server와 WAS의 차이

![](https://imgur.com/TJZ8pfR.png)

## Web Server

**웹 서버**란 클라이언트가 요청한 정적인 컨텐츠를 `HTTP 프로토콜`을 통하여 제공해주는 서버이다.  

하드웨어적으로 Web 서버가 설치되어 있는 컴퓨터를 의미한다.  
소프트웨어적으로 클라이언트로부터 HTTP 요청을 받아 정적인 컨텐츠를 제공하는 컴퓨터 프로그램이다.  

위에서 언급한 정적 페이지를 보내준다.  정적인 컨텐츠 제공이 가장 큰 역할이다.  

동적인 요청이 클라이언트로부터 들어왔을 때, 해당 요청을 웹 서버에서 처리할 수 없기 때문에 <U>컨테이너(Container)로 보내주는 역할</U>을 한다.  
- 예시: Nginx, Appach HTTP Server, IIS

1. 클라이언트 요청에 따라 정적인 컨텐츠를 제공한다.  
	- WAS를 거치지 않고 바로 자원을 제공한다.  
2. 클라이언트 요청에 따라 동적인 컨텐츠를 제공하기 위해 요청(Request)을 WAS에 보내고 WAS에서 처리한 결과를 클라이언트에게 전달(Response)하는 기능을 수행한다.  

![](https://imgur.com/KPk3xJS.png)  


## 웹 컨테이너 (Web Container/ Servlet Container)

Container는 동적인 데이터들을 처리하여 정적인 페이지로 생성해주는 모듈이다.  

- 예시
	- 사용자가 로그인해서 My Page 메뉴에 들어간다고 가정
	- 이 메뉴에서는 각자 사용자에 따라 보여질 정보가 다르다.  
	- 사용자의 요청이 들어오면 웹 서버는 정적인 요소만 클라이언트측에 보낼 수 있고, 동적으로 처리해야 하는 부분은 처리할 수 없다.   

컨테이너는 이러한 부분을 대신 처리해서 웹 서버에 정적인 파일로 만들어서 보내주는 모듈이라고 생각하면 된다.  

>[!info] CGI (Common Gateway Interface)란?
>인터페이스로서, 웹 서버 상에서 프로그램을 동작시키기 위한 방법을 정의한 프로그램이다.  
>- 웹 서버와 외부 프로그램 사이에서 정보를 주고 받는 방법이나 규약
>	- 두 개 이상의 컴퓨터 간의 자료들을 주고 받는 프로그램, 또는 주고 받는 것 자체를 의미한다.  

`PHP`, `Perl`, `Python`등의 언어는 Apache를 통해 CGI를 적용시키는 것이 가능하지만 `Java`는 안된다.  

`Java`는 따로 CGI와 같은 기능을 위해 컨테이너라는 것이 필요한데 그것이 servlet(서블릿)이다.  

![](https://imgur.com/owdxFuY.png)  

## Web Container 작동

1. 클라이언트는 웹 서버로 request(요청)을 보낸다.  
2. Servlet을 포함하는 WAS는 컨테이너로 요청을 보낸다.  
3. 컨테이너가 요청을 각 servlet에게 전달한다.  
4. servlet method가 load된다.  
5. Servlet은 컨테이너에 관련 response(응답)을 넘겨준다.  
6. 컨테이너는 이를 서버에 전달한다. 서버는 응답을 클라이언트에게 전달한다.  

## WAS(Web Application Server) 

웹 서버로부터 오는 동적인 요청을 처리하는 서버이다. 웹 서버와 컨테이너를 붙여놓은 서버라고 보면 된다.  

DB 조회 및 다양한 로직 처리 요구시 동적인 컨텐츠를 제공하기 위해 만들어진 애플리케이션 서버이다. HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어(소프트웨어 엔진)이다.  

- Tomcat, WebLogic, WebSphere, Jeus, JBoss 등이 있다. 

### 예시
클라이언트에서 `http://caffelove.com`이라는 도메인을 가진 서버에서 '내 정보'를 눌러 `http://caffelove.com/myinfo`라는 경로에 들어간다고 가정한다.  

`/myinfo`라는 경로로 요청하면 WAS는 자신의 라우팅 정보를 통하여 어떤 처리를 해야 될지 살펴본다.  
이때, `myinfo`를 라우팅할 때, 단순히 "myinfo.html이라는 파일을 보여줘" 라는 요청을 하게 된다면 정적인 요소이기 때문에, 웹 서버에서 클라이언트에게 myinfo.html 파일을 보내주기만 하면 된다.  

하지만, myinfo는 개인의 고유한 정보를 보여주는 페이지이니 WAS에서는 <U>데이터베이스에서 데이터</U>를 가져온다.  
그 다음에 원하는 데이터를 가공하여, 파일로 해당 데이터를 보내준다.  

![](https://imgur.com/dYWfM0D.png)  

### WAS의 동작 과정

![](https://imgur.com/kTmO4Bs.png)

1. 웹 서버에서 요청이 들어오면 Container가 요청을 받는다.  
2. 요청을 받은 Container는 배포서술자(web.xml)을 참조하여 해당 servlet에 대한 thread를 생성하고 요청(HTTPServletRequest) 및 응답(HTTPServletResponse) 객체를 생성하여 전달한다.  
3. Container는 사용자가 요청한 URL을 분석하여 어느 servlet에 대한 요청인지 찾는다.  
4. Container는 servlet을 호출(service())하며, POST/GET 여부에 따라 doPost() 또는 doGet()이 호출된다.  
5. 호출된 메서드는 동적 페이지를 생성한 후 HTTPServletResponse 객체에 실어 컨테이너 전달한다.  
6. Container는 전달 받은 객체를 웹 서버에 전달하여 생성되었던 thread를 종료하고 요청(HTTPServletRequest) 및 응답(HTTPServletResponse) 객체를 소멸시킨다.  

![](https://imgur.com/e3OEZVP.png)  

## Web Server가 필요한 이유

클라이언트(웹 브라우저)에 이미지 파일(정적 컨텐츠)을 보내는 과정을 생각해 본다면

- 이미지 파일과 같은 정적인 파일들은 웹 문서(HTML 문서)가 클라이언트로 보내질 때 함께 가는 것이 아니다.  

- 클라이언트는 HTML 문서를 먼저 받고 그에 맞게 필요한 이미지 파일들을 다시 서버로 요청하면 그때서야 이미지 파일을 받아온다.  

- Web Server을 통해 정적인 파일들을 Application Server까지 가지 않고 앞단에서 빠르게 보내줄 수 있다.  

📌**따라서, Web Server에서는 정적인 컨텐츠만 처리할 수 있도록 기능을 분배하여 서버의 부담을 줄일 수 있다.**  

## WAS가 필요한 이유

웹 페이지는 정적 컨텐츠와 동적 컨텐츠가 모두 존재한다.  

사용자의 요청에 맞게 적절한 동적 컨텐츠를 만들어서 제공해야 한다.  
이때, Web Server만을 이용한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어 놓고 서비스를 해야 한다. 

하지만 이렇게 수행하기에는 자원이 절대적으로 부족하다.

📌따라서, WAS를 통해 요청에 맞는 데이터를 **DB에서 가져와서 비즈니스 로직에 맞게 그때 그때 결과를 만들어서 제공함**으로써 자원을 효율적으로 사용할 수 있다.  

## Web Server + WAS 조합

각 둘의 장단점을 봤을 때 WAS와 Web Server의 기능을 모두 수행하면 최적의 서버를 구성할 수 있게 된다.  

1. 기능을 분리하여 서버 부하 방지
	- WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기 때문에 단순한 정적 컨텐츠는 Web Server에서 빠르게 클라이언트에 제공하는 것이 좋다.  
	- WAS는 기본적으로 동적 컨텐츠를 제공하기 위해 존재하는 서버이다.  
	- 만약 정적 컨테츠 요청까지 WAS가 처리한다면 정적 데이터 처리로 인해 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 수행 속도가 느려진다.  

2. 물리적으로 분리하여 보안 강화
	- SSL에 대한 암복호화 처리에 Web Server를 사용한다.  

3. 여러 대의 WAS를 연결 가능
	- fail over(장애 극복), fail back 처리에 유리하다.  
	- 대용량 웹 애플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.  
	- 예시로 앞 단의 Web Server에서 오류가 발생한 WAS를 이용하지 못하도록 한 후 WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있다.  

4. 여러 웹 애플리케이션 서비스 가능
	- 예시로 하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우

5. 기타
	- 접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server에서 처리하면 효율적이다.  
	- **자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성**을 위해 Web Server와 WAS를 분리한다.  
	- Web Server를 WAS 앞에 두고 필요한 WAS들을 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 분산 처리가 가능하다.   

![](https://imgur.com/K8PhM0t.png)


>[!reference]
>[웹 서비스 구조 (Web서버 / 웹 컨테이너/ WAS) 정리](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-%EC%9B%B9-%EC%84%9C%EB%B9%84%EC%8A%A4-%EA%B5%AC%EC%A1%B0-%EC%A0%95%EB%A6%AC#static_vs_dynamic_%ED%8E%98%EC%9D%B4%EC%A7%80)  
>[Web Server와 Web Applicaton Server](https://dev-donghwan.tistory.com/90)
