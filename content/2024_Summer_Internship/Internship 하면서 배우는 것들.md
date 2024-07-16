---
title: Learning from internship
date:
---

# Cloudflare

- Cloudflare Workers & Pages 에 들어가서 설정 가능
- 자동으로 obsidian에서 commit을 하게 되면 사이트에도 자동으로 배포가 됨

# Obsidian

- 형식 문서를 만들어 Template 폴더에 넣고 파일을 만든 뒤 단축키로 template을 지정하여 형식을 쓸 수도 있고 설정을 하여 새로 파일을 만들면 자동으로 그 형식으로 적용된다.
- Obsidian에서 자동으로 commit을 설정하면 Git에 저장이 되고 다른 원격 컴퓨터에서 Git을 통해 이용가능
이것은 문단 참조를 하기 위한 예시 문단입니다.^129389 ^f407c7

# jupyter notebook 에서 md로 바꿔주는 방법

- nbconvert 모듈 이용
- 코드를 만드는 칸 키를 사용하면 만들 수 있다 그 뒤에 사용하는 언어를 적으면 색깔로 표시가 된다.
- Ex)

```python
str1 = list(input())
num = int(input())

print(str1[num])
```

`사용하는 방법` 
# 이미지를 이곳에 적용하는 방식

`![]()`

- ! : 처음에 위치한다
- [] : 이건 이미지에 마우스 커서를 갖다 댈 때 이미지 주석을 넣는 곳
- () : 이건 이미지 주소를 넣는 곳 (이미지 링크를 생성하는 곳 imgur )
	- 마지막에 .png를 달아준다.
- Ex)
  ![](https://imgur.com/GYIKo4T.png)
---
#  Git  글자 표시
vs code에서 작업하는 폴더에서 git init을 하고 코딩을 하다보면 폴더와 파일 옆에 색깔있는 점이나 글자가 표시된다  


A - Added (This is a new file that has been added to the repository)  
저장소에 새로 추가된 것들

M - Modified (An existing file has been changed) : 갈색 점 표시  
수정된 것들

D - Deleted (a file has been deleted)  
삭제된 것들

U - Untracked (The file is new or has been changed but has not been added to the repository yet) : 녹색 점 표시  
새로 추가되거나 수정된 것들인데 아직 git add 안되서 추적이 안되는 상태

C - Conflict (There is a conflict in the file)

R - Renamed (The file has been renamed)

S - Submodule (In repository exists another subrepository)

빨간 점 : problem decorations를 표시하는것, 해당 폴더나 파일안에 코드 문법이나 다른 오류가 있다는 의미

git commit이 정상적으로 되면 이러한 표시들이 사라진다.

[연구원님 깃허브](https://narae3759.github.io/)

[옵시디언 무료 퍼블리시 방법](https://anpigon.tistory.com/m/449)

---
# Github에 잘 정리해야하는 이유

네이버 음식점 리뷰 프로젝트 마무리를 하며 연구원님께 데이터 추출, 데이터 전처리, 데이터 분석을 한 코드들을 모두 한 `.py`파일에 모으는 건지에 대해서 여쭈어 보았다.  

연구원님은 프로젝트를 보여주거나 제출할 때에는 Github 링크를 보낸다고 하셨다.  
그렇다면 Github 프로젝트에는 잘 정리가 되어있어야 한다는 뜻이었다.  

연구원님의 프로젝트를 보면
>[!example]- 연구원님의 프로젝트 Tree1
>![](https://imgur.com/g683PKj.jpg)

`review_analyzer` 폴더를 확인하면 사용한 tool에 대해 따로 파일을 만들어 놓은 것을 볼 수 있다. 이를 통해 import를 하여 사용하면 좀 더 편하게 사용할 수 있다.  
![연구원님이 import하여 쓰셨다.](https://imgur.com/ESWc9gu.jpg)

연구원님은 나중에 다른 프로젝트에서도 쓰일 코드는 `utils.py`로 따로 만들어 나중에도 편하게 쓰일 수 있게 하신다고 하셨다.
이러한 코드들이 어떤 걸 의미하는 코드인지 알아두는 것도 중요하다. 
>[!example]- 연구원님의 프로젝트 Tree2
>![](https://imgur.com/G7cVFzQ.jpg)

무조건 파이썬 파일로 만들어서 한 곳에 모아두는 것은 의미가 없다.  
내가 **정리한 것을 이해할 수 있게 만드는 것**이 중요하다.

*Jupyter Notebook을 사용하는 이유*
1. 흐름대로 바로 찾아가기
	- 만약 파이썬 파일로 저장을 했을 경우 긴 코드라면 내가 재사용하고 싶은 코드를 찾는데 오래 걸릴 수 있다.
	- 이를 좀 더 쉽게 찾아주는게 Jupyter Notebook이다. 
	- 연구원님이 `markdown`과 `jupyter notebook`의 순서를 같게 하라는 이유
		- 나중에 코드 또는 정리를 좀 더 빠르게 찾기 위함도 있다.
2. 면접용
	- 면접용은 내가 정리해둔 것에 대해 물어보는 것에 대한 준비를 할 경우에 대비하는 것이다.
		- 위의 흐름대로 찾는것과 마찬가지로 쉽게 보기 위함이 크다.