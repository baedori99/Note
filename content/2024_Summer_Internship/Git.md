---
title: Git
create_date: 2024-07-26 15:07 - 2024-07-26 15:07
draft: true
---
![900](https://imgur.com/iYLihJg.png)

# Git 기본 이해하기

## 처음 시작

- Local Repository(=로컬 저장소): 
	- 내 컴퓨터 안에 있는 저장소
	- 이 저장소를 작업할 때는 네트워크가 필요하지 않다.

- Remote Repository(=원격 저장소):
	- 원격 서버에 있는 저장소
	- 서버에 저장해야 하기에 네트워크가 필요하다.
	- github, gitlab 등

예시를 통해 이해를 할려고 한다. 
### 로컬 저장소와 원격 저장소 연결하기(git remote)

먼저 로컬 컴퓨터에 local repository를 만든다.  

1. `Test-git`이라는 폴더를 만든다.  
2. `Test-git`폴더에서 마우스 오른쪽 클릭으로 git bash로 들어간다.  
3. `git init` 명령어를 입력한다.  
![](https://imgur.com/yCh9DYA.png)
- `Test-git`폴더 안에 `.git`폴더가 생성이 된다.  

remote repository를 만든다.
- [Github Test-git](https://github.com/baedori99/Test-git) 

#### Local repository와 Remote repository 연결하기

1. Remote repository에 들어가 Code를 누른 후 HTTPS 주소를 복사한다.  
2. 아까 local repository를 만든 git bash에 돌아와 아래의 명령어를 적는다.  
		`git remote add origin "원격 저장소 주소"`  
>[!info]
>- origin: 내가 저장하고 관리하는 저장소
>- upstream: 여러명이 관리하는 저장소

위의 명령어는 remote origin (내가 저장하고 관리하는 원격 저장소)를 local origin에 복제한다는 의미이다.  
![](https://imgur.com/aADXlf8.png)

3. 연결이 되었으니 아래 명령어를 통해 remote repository에 있는 파일을 불러와 local repository에 복제를 한다.  
		`git pull origin main`  

remote repository에 있는 파일이 local repository에 있는 것을 확인 할 수 있다.
![](https://imgur.com/XVFmM29.png)

## 로컬 저장소에 Remote repository 생성하기

1. Remote repository를 생성한다.  
2. remote repository를 복사할 로컬 저장소 위치를 정하고 git bash 또는 터미널을 연다.  
3. 아래의 명령어를 친다.
		`git clone "원격 저장소 주소"`  
4. 로컬 저장소에 remote repository가 복사된 것을 확인할 수 있다.

# 🔥 무조건 알아둬야할 Git 사용 순서

프로젝트 또는 다른 걸 하더라도 까먹지 말고 해야하는 행동이 있다.

1. Pull
	- pull로 파일에 대한 update를 한다
	- 혹시 충돌이 일어날 수 있으니 반드시 하자
2. add
	- add로 Staging Area에 파일을 넣는다.
3. commit
	- Staging Area에서 저장을 한다.
	- 커밋 메시지를 통해 파일의 변경점에 대해 설명한다.
4. push
	- Remote repository(원격 저장소)에 저장한다.
5. pull request
	- 다른 branch에 있다면 branch를 최신 시키기 위해 merge하기 전에 확인하는 용
6. merge
	- 파일의 변경점을 합쳐 최신 상태를 유지한다.