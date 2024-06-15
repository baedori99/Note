---
title: Important Things with using OPEN API
---
[[Urllib에 관하여]]

>[!important]
> Python에서 중요한 정보(API KEY, ACCESS KEY등의 중요한 정보)를 숨기는데 쓰이는 .env와 .gitignore에 대해 알아보자

### OPEN API를 이용하며 제일 중요한 것
### KEY
- OPEN API를 이용하면서 쓰이는 인증키가 제일 중요한 요소중 하나이다.
**※ KEY를 Github에 올리게 될 경우 엄청난 불상사를 겪을 수 있다.**
- 이러한 불상사를 막기 위해 쓰이는 것은 python에서는 `.env`라는 파일과 `.gitignore`이다.
#### .env
- .env 파일은 외부로 노출이 되어서는 안되는 변수들을 모아두는 파일이다.
#### .gitignore
- .gitignore는 Github로 푸시가 안되는 파일을 말한다. 

#### 만약 .gitignore에 넣기전에 .env을 Github로 push했다면?
만약 위와 같은 경우나 .gitignore에 등록했는데 커밋목록에 나타났다면
.gitignore에 파일을 추가하기 전에 stage에 올라간 파일들은 **캐시가 남아 있어** 커밋모록에 뜨는 것이다.
- 캐시를 제거하기 위해서는 **해당 디렉토리로 이동 후** 명령어를 입력해주면 된다.

```
// 캐시를 모두 삭제
git rm -r --cached .

// .gitignore에 입력된 파일 목록을 제외한 다른 모든 파일을 다시 트레킹
git add .

// 커밋
git commit -m "clear git cache"
```
