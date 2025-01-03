# training_frontend
# 한 번에 끝내는 프론트엔드 개발 초격차 패키지 Online. 과정 실습

github이 public리포지터리를 지원하지 않던 시절 bitbucket만 사용했는데 이제는 github을 적극 활용예정입니다.

# 마크다운 문법 연습

# 제목(Header)
# 제목 #1개
## 제목 #2게
### 제목 #3개
#### 제목 #4개
##### 제목 #5개
###### 제목 #6개
####### 제목 (#6개 까지만 지원. 7개X)

# 문장(Paragraph)
동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세

# 줄바꿈(Line Breaks)
원래는 문장 끝에 스페이스 두번 후 엔터로 줄바꿈이 되어야 하나 안되는 경우 \<BR> 브레이크 태그 사용해도 됨.  
동해물과 백두산이 마르고  
닳도록 하느님이 보우하사  
우리나라 만세  

# 강조(Emphasis)
_이텔릭_  
**두껍게**  
**_이텔렉+두껍게_**  
<u>밑줄</u>  

# 목록(List)
1. 순서가 필요한 목록
1. 순서가 필요한 목록
1. 순서가 필요한 목록
1. 순서가 필요한 목록
1. 순서가 필요한 목록

- 순서가 필요하지 않은 목록
- 순서가 필요하지 않은 목록
- 순서가 필요하지 않은 목록
- 순서가 필요하지 않은 목록

# 링크(Links)
마크다운에서는 타겟에 _blank를 줄 수는 없으므로 필요한 경우 원시html을 사용해야함  
[GOOGLE](https://google.com)

# 이미지(Images)
![MY_SON](https://raw.githubusercontent.com/foenix76/training_frontend/refs/heads/main/son_young.jpg)  
이미지 + 링크  
[![MY_SON](https://raw.githubusercontent.com/foenix76/training_frontend/refs/heads/main/son_young.jpg)](https://dev.hi.ne.kr)

마크다운의 한계 : 이미지 크기 조절을 위해서는 원시태그를 사용할 수 밖에 없음  

# 인용문(BlockQuote)
> 남의 말이나 글에서 직접 또는 간접으로 따온 문장.
>(네이버 국어 사전)
>> 중첩 가능 2
>>> 중첩 가능 3

# 인라인(inline) 코드 강조
CSS에서 `background` 혹은 `background-image` 속성으로 요소에 배경 이미지를 삽입할 수 있습니다.

# 블록(block) 코드 강조
백틱 3개 + 포메팅을 원하는 언어. javascript, bash 등
```html
<a href="https://google.com">GOOGLE</>
```
지원안하는 경우 plaintext  
```plaintext
지원안하는 언어입니다. 
```    

# 표(Table)
기본은 왼쪽 정렬이며 콜론으로 양쪽을 감싸면 중앙정렬, 콜론1개를 우측에 넣으면 우측정렬됨  
마크다운의 한계 : html테이블처럼 복잡한 테이블은 만들 수 없음

position 속성
값 | 의미 | 기본값
--|:--:|--:
static | 기준 없음 | O
relative | 요소 자신 | X
fixed | 뷰포트 | X

# 원시 HTML (Raw HTML)
마크다운 해석이 이상하게 되는 환경이거나 마크다운으로 할 수 없는 것을 보여줘야 할 때는 일반 HTML로 할 수 있다.

# 마크다운을 공부해보며 느낀 점
항상 별 생각없이 md파일을 읽기만 했는데 직접 문법을 공부해보니 상당히 편리하게 그럴듯한 문서를 만들 수 있다는 것을 알게 되었다.
