---
title: Learning from internship
date: <%= tp.date.now('2024-06-07') %>
---

# Cloudflare

- Cloudflare Workers & Pages 에 들어가서 설정 가능
- 자동으로 obsidian에서 commit을 하게 되면 사이트에도 자동으로 배포가 됨

# Obsidian

- 형식 문서를 만들어 Template 폴더에 넣고 파일을 만든 뒤 단축키로 template을 지정하여 형식을 쓸 수도 있고 설정을 하여 새로 파일을 만들면 자동으로 그 형식으로 적용된다.
- Obsidian에서 자동으로 commit을 설정하면 Git에 저장이 되고 다른 원격 컴퓨터에서 Git을 통해 이용가능

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

![]()

- ! : 처음에 위치한다
- [] : 이건 이미지에 마우스 커서를 갖다 댈 때 이미지 주석을 넣는 곳
- () : 이건 이미지 주소를 넣는 곳 (이미지 링크를 생성하는 곳 imgur )
	- 마지막에 .png를 달아준다.
- Ex)
  ![](https://imgur.com/GYIKo4T.png)

# 테스트용
