---
title: Git
create_date: 2024-07-26 15:07 - 2024-07-26 15:07
draft: true
---
![900](https://imgur.com/iYLihJg.png)

# Git 기본 이해하기

![|800](https://imgur.com/9GArLFm.png)

## ① 처음 시작

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

### 로컬 저장소에 Remote repository 생성하기

1. Remote repository를 생성한다.  
2. remote repository를 복사할 로컬 저장소 위치를 정하고 git bash 또는 터미널을 연다.  
3. 아래의 명령어를 친다.
		`git clone "원격 저장소 주소"`  
4. 로컬 저장소에 remote repository가 복사된 것을 확인할 수 있다.

## ② 스테이징 영역 (Staging Area)

저장소에 커밋하기 전에 **커밋을 준비하는 위치**  

- 예시
	- 작업 트리에서 10개의 파일을 수정했는데 4개의 파일만 버전으로 만들려면 4개의 파일만 스테이지로 넘겨주면 된다.  
- 로컬 스테이지에 올려둔 파일만 원격 저장소에 커밋할 자격이 있는 것이다.

## ③ 커밋 (Commit)

스테이징 영역에서 로컬 저장소로 저장하는 것


# 🔥 무조건 알아둬야할 Git 사용 순서

프로젝트 또는 다른 걸 하더라도 까먹지 말고 해야하는 행동이 있다.

## 1. Pull
- pull로 파일에 대한 update를 한다
- 혹시 충돌이 일어날 수 있으니 반드시 하자

## 2. add
- add로 Staging Area에 파일을 넣는다.  
- 쓰는 법
	- `git add .`: 모든 변경 파일을 Staging Area에 전송한다.
	- `git add [파일 이름]`: 특정 파일만 Staging Area로 전송한다.

## 3. commit
- 현재 변경된 작업 상태를 점검을 마치면 확정하고 **저장소에 저장하는 작업**
- 커밋 메시지를 통해 파일의 변경점에 대해 설명한다.
- 쓰는 법
	- `git commit -m "커밋 메시지"`: 한 줄로 커밋의 내용을 작성하는 것이다.  
		- `-m`은 한 줄이라는 의미로 쓰였다.
	- `.gitmessage.txt`를 커밋 메시지 템플릿으로 사용할 수 있다.
		- 템플릿이 있을 시 `git commit`만 사용해서 커밋 메시지 사용 가능
			1. **그전에** `git config core editor "code --wait"`로 git editor를 vscode로 설정한다.
			2. `.gitmessage.txt`를 생성하고 템플릿을 작성한다.
			3. `git config --global commit.template .gitmessage.txt`를 `cmd`에 친다.
			4. `git config --list`로 설정되었는지 확인한다.

## 4. push
- Remote repository(원격 저장소)에 저장한다.

## 5. pull request
- 다른 branch에 있다면 branch를 최신 시키기 위해 merge하기 전에 확인하는 용

## 6. merge
- 파일의 변경점을 합쳐 최신 상태를 유지한다.


>[!reference]
>[Git 공식 문서](https://git-scm.com/book/ko/v2)
>[git 개념 & 원리 (그림으로 알기 쉽게 비유)](https://inpa.tistory.com/entry/GIT-%E2%9A%A1%EF%B8%8F-%EA%B0%9C%EB%85%90-%EC%9B%90%EB%A6%AC-%EC%89%BD%EA%B2%8C%EC%9D%B4%ED%95%B4)

