---
title: Obsidian Markdown 사용방법
create_date: 2024-06-14 16:06 - 2024-06-14 16:06
draft: true
---
# 마크다운 문법 정리

## [Obsidian Help](https://publish.obsidian.md/help-ko/%ED%99%88)

### 1. 제목(Headers)
- 제목을 만들 때는 `#`을 사용하며, `#`의 개수에 따라 제목의 수준이 달라집니다.

```
# 제목 1 (가장 큰 제목)
## 제목 2
### 제목 3
#### 제목 4
```

- 결과:

![](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-10.png)

### 2. 굵게(Bold)
- 텍스트를 굵게 표시하려면 `**굵게**` 또는 `__굵게__`를 사용하여 텍스트를 감싸면 됩니다.

```
**굵은 텍스트**
__굵은 텍스트__
```

- 결과:
	- **굵은 텍스트**
	- __굵은 텍스트__
### 3. 기울임, 이탤릭(Italic)
- 텍스트를 기울임 (이탤릭) 체로 표시하려면 `*` 또는 `_`로 텍스트를 감싸면 됩니다. 앞에서 굵게 하기 위해서는 2개, 기울이는 것은 1개만 사용하는 것이 차이점입니다.

```
*이탤릭 텍스트*
_기울임 텍스트_
```

- 결과: 
	- *이탤릭 텍스트*
	- _기울임 텍스트_
### 4. 취소선(Strikethrough)
- 텍스트에 취소선을 추가하려면 `~~`로 텍스트를 감싸면 됩니다.

```
~~~취소선 텍스트~~~
```

- 결과: 
	- ~~~취소선 텍스트~~~ 

### 5. 리스트(Lists)
- 순서가 있는 리스트와 순서가 없는 리스트를 작성할 수 있습니다.

#### 순서가 있는 리스트
- 숫자와 점을 사용하여 순서가 있는 리스트를 작성할 수 있습니다.

```
1. 항목 1
2. 항목 2
3. 항목 3
```

- 결과:
	1. 항목 1
	2. 항목 2
	3. 항목 3

#### 순서가 없는 리스트
- `-`,`+`,`*`를 사용하여 순서가 없는 리스트를 작성할 수 있습니다.

```
- 항목 1
- 항목 2
- 항목 3
```

- 결과:
	- 항목 1
	- 항목 2
	- 항목 3

### 6. 링크(Links)
- 링크를 삽입하려면 `[링크 텍스트](URL)`형식을 사용합니다. 결과 링크의 맨 오른쪽에 붙은 아이콘이 현재 옵시디언 바깥으로 나가는 링크가 삽입되어있다는 의미를 나타냅니다.

```
[뚝바이브 개발자](https://baedori99.pages.dev/)
```

- 결과:
	- [뚝바이브 개발자](https://baedori99.pages.dev/)

### 7. 이미지(Images)
- 이미지를 삽입하려면 `![대체 텍스트](이미지 URL)` 형식을 사용합니다.
- 이미지 주소를 만드는 곳은 [imgur](https://imgur.com/)

```
![위키피디아 흑요석](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)
```

- 결과:
	![위키피디아 흑요석](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-16-300x243.png)

### 8. 인용구(Blockquotes)
- 인용구를 만들려면 `>`를 사용합니다. 또한 백슬래쉬(`\`)을 사용하면 마크다운 문법에 사용되는 기호들을 사용할 수 있는데요, 아래 예제에서는 리스트를 만드는 `-`기호를 백슬래쉬 기호를 사용하여 삽입한 것을 볼 수 있습니다.

```
> Try not to become a man of success but rather try to become a man of value.

\- Albert Einstein
```

- 결과:
	> Try not to become a man of success but rather try to become a man of value.

\- Albert Einstein

### 9. 코드(Code)
- 인라인 코드는 `'backticks'`로 감싸고, 코드 블록은 3개의 `'backticks'`로 감싸서 작성합니다.

```
인라인 코드: `print("Hello, World")`

코드 블록:
```
```
'''
def greet():
	print("Hello, World")
'''
```

- 결과:
인라인 코드: `print("Hello, World")`
코드 블록:
```
def greet():
	print("Hello, World")
```

### 10. 수평선(Horizontal Rule)
- 수평선을 추가하려면 `---`,`___`,또는 `***`을 사용합니다.

```
---
***
___
```

- 결과
---
***
___

### 11. 각주(Footnotes)
- 각주를 노트에 추가하는 방법은 아래와 같습니다.
-  `ctrl + E`를 눌러 각주 확인 가능

#### 기본 각주 문법
- 문장에 각주를 추가하려면 `[^1]`와 같은 방식으로 텍스트에 각주를 표시하고, 페이지 하단에 해당 각주에 대한 설명을 작성할 수 있습니다.

```
이것은 간단한 각주[^1]입니다.

[^1]: 이것은 참조된 텍스트입니다.
```

- 결과:
이것은 간단한 각주[^1] 입니다.
[^1]: 이것은 참조된 텍스트입니다.	

#### 여러 줄의 각주 작성
- 각주가 여러 줄에 걸쳐 작성되어야 하는 경우 각 새 줄 앞에 2개의 공백을 추가합니다.

```
이것은 각주가 여러줄로 달리는 경우[^2]입니다.

[^2]: 각주의 첫 줄입니다.
	이것은 각주가 여러줄에 걸쳐 작성될 때 사용하는 방법입니다.
```

- 결과:
이것은 각주가 여러줄로 달리는 경우[^2]입니다.

[^2]: 각주의 첫 줄입니다.
	이것은 각주가 여러줄에 걸쳐 작성될 때 사용하는 방법입니다.

#### 이름이 지정된 각주
- 각주에 이름을 지정하여 각주를 더 쉽게 식별하고 참조를 연결할 수 있습니다. 이름이 지정된 각주도 번호로 표시됩니다.

```
이것은 이름이 지정된 각주[^note]입니다.

[^note]: 이름이 지정된 각주는 숫자로 나타나지만 식별과 참조가 더 쉬울 수 있습니다.
```

- 결과
이것은 이름이 지정된 각주[^note]입니다.

[^note]: 이름이 지정된 각주는 숫자로 나타나지만 식별과 참조가 더 쉬울 수 있습니다.

---
# 옵시디언 전용 마크다운 문법 정리

### 1. 하이라이트 (Highlight)
- 마크다운에는 기본적으로 텍스트 하이라이트 기능이 없습니다. 그러나, 옵시디언에서는 `==` 를 사용하여 텍스트를 하이라이트 할 수 있습니다.

```
==하이라이트 텍스트==
```

- 결과:
	==하이라이트 텍스트==

### 2. 체크박스 (Check Box)
- 마크다운으로 체크 박스나 체크 리스트를 삽입하려면 각 목록 항목을 하이픈(`-`)과 공백 뒤에 `[]`로 시작하시면 됩니다. 공백 안에 x표시를 할 경우, 목록에 취소선이 생깁니다. 공백 안을 다른 문자로 채우게 되면, 완료 표시로 바뀌게 됩니다.

```
### 해야 할 일들

- [x] 화분에 물주기
- [-] 우편함 확인
- [ ] 밀린 일기 작성
```

- 결과:
`### 해야 할 일들`

- [x] 화분에 물주기
- [-] 우편함 확인
- [ ] 밀린 일기 작성

#### 참고: 옵시디언 체크박스 단축키
- 옵시디언에서는 체크 박스를 쉽게 만들 수 있도록 단축키가 기본으로 제공됩니다. 체크박스를 만들기 원하는 행에 커서를 놓고 `Ctrl + L` 단축키를 누르게 되면 자동으로 체크 박스로 변하게 됩니다.

### 3. 그림 크기 (Image Size)
- 압에서 설명한 일반적인 마크다운 이미지 삽입 코드에 옵시디언에서만 작동하는 이미지 크기를 조절하는 옵션이 존재합니다. `![대체 텍스트|가로x세로](이미지URL)`형식을 사용합니다.

```
![위키피디아 흑요석|100x100](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)
```

- 결과:
![위키피디아 흑요석|100x100](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)

### 4. 문서 링크 (Internal Links)
- 내부 링크는 노트 간 또는 블록 간에 서로 연결되는 링크입니다. `[[]]`안에 페이지 이름을 넣어서 생성할 수 있습니다. 이를 통해 빠르게 관련 정보로 이동할 수 있습니다. 예를 들어, Vault안에 "Internship 하면서 배우는 것들"이라는 노트가 있는 경우, 다음과 같이 작성하면, 옵시디언 사용법 노트로 가는 링크가 생성됩니다.

```
[[Internship 하면서 배우는 것들]]
```

- 결과:
	[[Internship 하면서 배우는 것들]]

### 5. 문서 임베딩 (Embedding Links)
- 내부 링크와는 다르게 노트 내용 자체를 다른 노트에 박아놓을 때 사용하는 마크다운 문법입니다. 알고 있으면 상당히 유용합니다. 아래 결과를 보면 현재 노트에 다른 노트가 임베딩 되어 있다는 표시가 되어 있는 것을 확인 할 수 있습니다.

```
![[Internship 하면서 배우는 것들]]
```

- 결과:
	![[Internship 하면서 배우는 것들]]
### 6. 문단 참조 (Block Reference)
- 블록 참조는 다른 블록 내용을 현재 블록에 포함시키는 기능입니다. `![[#^id]]` 형식으로 사용합니다.

```
![[Internship 하면서 배우는 것들#^myobsi]]
```

- 결과:
![[Internship 하면서 배우는 것들#^f407c7]]

### 7. 강조 상자 넣기 (Callout, 말풍선)
- 내용을 쓰다가 강조하는 부분이 있다거나, 정리를 할 때 유용한 문법입니다.

```
> [!note] 이것만 알아두세요!
> 옵시디언은 정말 편한 도구입니다.
```

>[!note] 이것만 알아두세요!
>옵시디언은 정말 편한 도구입니다.

#### 콜아웃 블록 접기 기능
- 콜아웃의 강점은 접기 기능이 있다는 점입니다. 옵시디언의 경우 제목을 접을 수 있지만 **콜아웃 블록과 차이점은 초기값을 지정할 수 있다는 점**입니다.
- 강조하고 싶은 내용을 콜아웃 블록에 표현하거나 추가 메모를 추가할 수도 있지만, 내용이 길면 처음에는 닫아 두는 것이 좋습니다.
- 콜아웃 블록의 첫번째 줄에 있는 유형 옆에 하이픈(`-`)을 추가하여 초기 값이 닫힘인 콜아웃 블록을 생성할 수 있습니다.

```
> [!success]- Callouts block - Initially folded callouts block
> I am a callouts of type success
```

- 결과:
> [!success]- Callouts block - Initially folded callouts block
> I am a callouts of type success

#### 콜아웃 블록 유형
- 옵시디언에서 미리 정의된 콜아웃 블록의 색상과 아이콘은 다음과 같습니다.
- 콜아웃 유형에 원하는 텍스트를 작성하여 콜아웃의 모양을 변경할 수 있습니다.

![](https://unclesnote.com/assets/images/231128001800/unclesnote-obsidian_callouts_and_quote_block-txt_obsidian-callouts_predefined_types.png)

```
> [!quote] quote

> [!abstract] abstract, summary, tldr

> [!important] tip, hint, important

> [!note] note

> [!info] info

> [!todo] todo

> [!example] example

> [!question] question, help, fnq

> [!warning] warning, caution, attention

> [!failure] failure, fail, missing

> [!danger] danger, error

> [!bug] bug
```

- 결과:
> [!quote] quote

> [!abstract] abstract, summary, tldr

> [!important] tip, hint, important

> [!note] note

> [!info] info

> [!todo] todo

> [!example] example

> [!question] question, help, fnq

> [!warning] warning, caution, attention

> [!failure] failure, fail, missing

> [!danger] danger, error

> [!bug] bug

