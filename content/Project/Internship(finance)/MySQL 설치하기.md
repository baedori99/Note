---
title: MySQL 설치하기
create_date: 2024-07-31 14:07 - 2024-07-31 14:07
draft: true
---
MySQL을 이미 설치하고 난 뒤에 나중을 위해 작성하는 노트이기에 블로그 사진으로 사진을 대체하도록 하겠다.  
## MySQL을 설치를 위한 컴퓨터 환경

MySQL Community 8.0을 설치할 하드웨어에는 Windows만 설치되어 있다면 특별한 제한이 없다.  하지만 Window 운영 체제는 64bit Windows 10(또는 11)이 설치되어 있어야 한다.  

프로그램을 설치하기 전에 컴퓨터의 사양및 운영체졔를 확인한다.  

## MySQL 다운로드 및 설치하기  

### MySQL 다운로드

1. 웹 브라우저를 실행하여 [MySQL 다운로드 사이트](https://dev.mysql.com/downloads/installer/)에 들어가 용량이 큰 파일을 다운로드 받으면 된다.  2024-08-26 기준으로 8.0.39 버전이다.  
![](https://hongong.hanbit.co.kr/wp-content/uploads/2021/10/MySQL_Installer.png)  

### MySQL 설치하기

1. 다운로드한 파일을 더블 클릭해서 설치를 시작한다. 잠시 기다리면 로고가 잠깐 나타났다 사라진다. 사용자 계정 컨트롤 창이 나타나면 `예` 버튼을 클릭한다.  
	- 경우에 따라 `License Agreement`창이 나타날 수도 있다.  (`I accept the license terms`)를 체크하고 `Next`버튼을 클릭한다.  

2. MySQL Installer 창이 나타난다. `Choosing a Setup Type`에서는 설치 유형을 선택할 수 있는데, 필요한 것들만 골라서 설치하기 위해 `Custom`을 선택하고 `Next`버튼을 클릭한다.  
![|500](https://hongong.hanbit.co.kr/wp-content/uploads/2021/10/MySQL_%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0.png)  


3. `Select Products and Features`에서는 설치할 제품들을 선택할 수 있다. 우선 `Available Products:`에서 `MySQL Servers` – `MySQL Server` – `MySQL Server 8.0` – `MySQL Server 8.0.39 – X64`를 선택하고 ▶버튼을 클릭한다.
![|500](http://hongong.hanbit.co.kr/wp-content/uploads/2021/10/MySQL_Installer_Select_Products_and_Features-e1635409866264.png)  


4. 같은 방식으로 다음 2개를 추가한다. 다음 그림과 같이 총 3개가 추가되었으면 [Next] 버튼을 클릭한다.

- ① `Applications` – `MySQL Workbench` – `MySQL Workbench 8.0` – `MySQL Workbench 8.0.21 – X64`  
- ② `Documentation` – `Samples and Examples` – `Samples and Examples 8.0` – `Samples and Examples 8.0.21 – X86`
![|500](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/MySQL_Installer_Select_Products_and_Features-800x381.png) 
📌만약 Check Requirement 창이 나타나면 `Execute`버튼을 클릭해서 필요한 부분의 설치를 진행한다.  


5. `Installation`에서 3개의 항목을 확인하고 `Execute` 버튼을 클릭해서 설치를 진행합니다. 각 항목의 `Progress`에 설치 진행 과정이 숫자(%)로 보입니다. 설치가 완료될 때까지 잠시 기다립니다.  
![|500](http://hongong.hanbit.co.kr/wp-content/uploads/2021/11/MySQL_Installer_Installation-e1635743862700.png)  


6. 설치가 성공적으로 완료되면 각 항목 앞에 초록색 체크가 표시되고 `Status`가 `Complete`로 변경된다. 추가 환경 설정을 위해 `Next`버튼을 클릭한다.  

7. `Product Configuration`에 2개 항목의 환경 설정이 필요하다고 나온다. `Next`버튼을 클릭한다.  
![|500](http://hongong.hanbit.co.kr/wp-content/uploads/2021/11/MySQL_Installer_Product_Configuration-e1635743982621.png)  


8. `Type and Networking`에서 `Config Type`을 ‘Development Computer’로 선택하고 `TCP/IP`가 체크된 상태에서 `Port`가 ‘3306’인 것을 확인한다. 이 번호는 자주 사용되므로 꼭 기억하도록 한다. 그 아래 `Open Windows Firewall ports for networkaccess`도 체크되어 있어야 한다. `Next` 버튼을 클릭한다.  
![500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJn2IR%2FbtstmGzB3oN%2F3lpc27pF1xEezx7X3ZwYYK%2Fimg.png)  

9. `Authentication Method`에서는 파이썬과의 연동을 원활하게 하기 위해 ‘Use Legacy Authentication Method’를 선택하고 `Next` 버튼을 클릭한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbRvlf5%2FbtstlfvBfub%2FzIyI7iT4kPPUYNzi3FjSB1%2Fimg.png)


10. `Accounts and Roles`에서는 MySQL 관리자(Root)의 비밀번호를 설정해야 한다. 나는 ‘1234’으로 지정했다. 아래쪽의 `MySQL User Accounts`에서 Root 외의 사용자를 추가할 수 있다. 지금은 그냥 비워 두고 `Next` 버튼을 클릭한다.
![500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FciO0HK%2FbtstkwEyf8t%2F0gDPtOQdlIRf8mHLzjOcb1%2Fimg.png)  
📌`Root`는 MySQL의 모든 권한이 있는 관리자의 이름이다. 이 관리자의 비밀번호가 유출되면 컴퓨터의 중요한 정보가 모두 유출될 수도 있으므로 `Root`의 비밀번호는 문자/숫자/기호를 섞어서 최소 8자 이상으로 만들 것을 권장한다. 지금은 학습 중이므로 기억하기 쉽게 ‘1234’으로 지정한 것뿐이다.  

11. `Windows Service`에서는 MySQL 서버를 윈도우즈의 서비스로 등록하기 위한 설정을 진행한다. `Windows Service Name`은 전통적으로 많이 사용하는 ‘MySQL’로 변경한다. 나머지는 그대로 두고 `Next` 버튼을 클릭한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzKzEE%2Fbtstq9HPRe7%2Fz8vV11NGkRa8bAt7B3AZYK%2Fimg.png)  


12. `Server File Permissions`에서는 서버 권한을 설정하는 항목이다. 첫번째 라디오버튼을 클릭하고 `Next`를 클릭한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbuTtl7%2FbtstqFttemT%2FlK5MKFbG8Krd4jew6jQO2k%2Fimg.png)  


13. `Apply Configuration`에서 설정된 내용을 적용하기 위해 `Execute` 버튼을 클릭한다. 각 항목에 모두 초록색 체크가 표시되면 `MySQL Server`에 대한 설정이 완료된 것이다. `Finish` 버튼을 클릭해서 설정을 종료한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F8vdQP%2FbtstsPPGqPP%2FzwOhuIrlVBK99xbXrri1L1%2Fimg.png)  


14. 다시 `Product Configuration`이 나타난다. MySQL Server 8.0.34은 설정이 완료되었으며, 두 번째 Samples and Examples 8.0.34의 설정을 할 차례이다. `Next` 버튼을 클릭한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWvpYH%2FbtstnwDEkmR%2Fwpg3zMys2fNxyRZG1REMU0%2Fimg.png)  


15. `Connect To Server`에 연결할 서버가 보이고 `User name(사용자 이름)`에 ‘root’가 입력되어 있다. `Password(비밀번호)`를 앞에서 설정한 ‘1234’으로 입력하고 `Check` 버튼을 클릭하면 `Status`가 ‘Connection succeeded’로 변경된다. 연결이 성공되었으니 `Next` 버튼을 클릭한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FS8zqv%2Fbtsts73Spvf%2Frx8LAsqxchmRZnvX8iTC2k%2Fimg.png)  


16. `Apply Configuration`에서 `Execute` 버튼을 클릭하면 설정된 내용이 적용된다. 모든 항목 앞에 초록색 체크가 표시되면 성공이다. Samples and Examples에 대한 설정이 완료되었다. `Finish` 버튼 클릭해서 설정을 종료한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmHe6f%2Fbtstnu6R5d1%2FXLX1qiH10hEXY4njmIv8t1%2Fimg.png)  


17. 다시 `Product Configuration`이 나온다. `Status`를 보면 모두 완료된 것이 확인된다. `Next` 버튼을 클릭한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPLJ1z%2Fbtstr2oan1q%2FTWo1pnYOJvL0KEQf1VTp7K%2Fimg.png)  


18. `Installation Complete`에서 `Start MySQL Workbench after Setup`을 체크 해제하고 `Finish` 버튼을 클릭한다. MySQL의 설치를 완료했다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGmIj6%2FbtstqZL3ste%2FGeMWkk7MWpZrU84ngYfl61%2Fimg.png)  


## MySQL 정상 작동 확인하기

- MySQL 설치를 완료했으니, 정상적으로 잘 작동하는지 확인한다.  

1.  작업 표시줄에 고정하기
- Windows의 `시작` 버튼을 클릭하고 `MySQL` – `MySQL Workbench 8.0 CE`에서 마우스 오른쪽 버튼을 클릭한 후 `자세히` – `작업 표시줄에 고정`을 선택한다. 작업 표시줄에 돌고래 모양의 아이콘이 추가된다.
![|200](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FYffHz%2Fbtsts4TAD0p%2FTTccd5QsDRNIgskkiP8V4k%2Fimg.png)  

2. MySQL Workbench ( ) 아이콘을 클릭해서 프로그램을 실행한다. MySQL Workbench(워크벤치) 창의 좌측 하단에서 `MySQL Connections`의 ‘Local instance MySQL’을 클릭한다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FVirFw%2FbtstkWJRAEQ%2FI1nezPLxK5kIryDq9qCKPK%2Fimg.png)  

3. Connect to MySQL Server 창이 나타난다. `User`는 ‘root’로 고정되어 있고 `Password`가 비어 있다. MySQL을 설치할 때 지정한 ‘1234’을 입력하고 `OK` 버튼을 클릭한다.
![|300](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbDbAhC%2FbtstqvdlT7a%2F2wjvuMCUoJ4msk4yBbkPl0%2Fimg.png)  

4. MySQL Workbench가 MySQL 서버에 접속된 화면이 나타난다. 초기 화면에 나타난 `SQL Additions` 패널은 사용할 일이 없다. 툴바 우측에 위치한 3개의 네모 모양 아이콘 중에서 SQL Additions ( ) 아이콘을 클릭하면 많은 자리를 차지하는 `SQL Additions` 패널은 숨길 수 있다.
![|300](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbDSJ1C%2FbtstmEIHwBG%2F17oskLf5V4Mkt8kBo76Lj1%2Fimg.png)

5. 최종적으로 완성된 MySQL Workbench 화면이다. 주로 이 화면을 사용하게 될 것이다. 가운데 빈 공간은 Query 창이라고 부르며 메모장처럼 글자를 입력할 수 있는데, 여기에 SQL을 입력하면 된다.
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fwz5lY%2FbtstmtUJw5e%2FiwM2yREeAxLYlvQ3OjMisk%2Fimg.png)  

6. 정상적으로 동작하는지 알아보기 위해 간단한 SQL을 입력해 본다. 빈 공간에 다음과 같이 입력한다.
```sql
SHOW DATABASES
```

- 그리고 Execute the selected portion of the script or everything( ) 아이콘을 클릭하면 아래쪽 `Result Grid` 창에 SQL에 대한 결과가 나온다. MySQL 서버에 기본적으로 들어 있는 데이터베이스의 목록을 출력해준 것이다. 
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbuq5AM%2FbtstrPJc0NS%2FCeUSDL2gP82YwwMBklULo1%2Fimg.png)  

7. 작업을 모두 마쳤다면 `SQL File 숫자` 탭의 닫기(X) 버튼을 클릭해서 창을 닫는다
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxUZqJ%2FbtstwWA5cqf%2Fr0DkxYUr7qrqAIgLIUlBF1%2Fimg.png)  


>[!reference]
>[혼공 MySQL 설치 방법과 정상 설치 확인하기](https://hongong.hanbit.co.kr/mysql-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C-%EB%B0%8F-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0mysql-community-8-0/)  
>[소연의_개발일지 MySQL 설치 방법과 설치 확인하기](https://giveme-happyending.tistory.com/203#article-1--%F0%9F%9B%A0-mysql-%EC%84%A4%EC%B9%98%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%BB%B4%ED%93%A8%ED%84%B0-%ED%99%98%EA%B2%BD)  