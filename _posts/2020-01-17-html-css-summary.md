---
title: "[교육후기]HTML/CSS 기초"
categories:
  - review
tags:
  - 교육
  - FE
---

# HTML/CSS 교육
`교육 내용 출처: NHN  기술교육 - JavaScript 기본 (김성호 책임)`

처음 조금 놓침
* meta tag: 페이스북에서 만들었고, og:url og:image og:title 이런식으로 광고? 링크같은 걸 만들어줌. Open Graph Protocol
* title: \<title>토스트 루키 7\</title>
* h1~h6: 제목이나 부제목을 표현. h1이 제일 크고 h6d이 제일 작음.
* p 태그: paragraph 의 약자로 일반적인 텍스트의 단락을 표현

```HTML/CSS\<p>P태그\</p>단락을 표현합니다.```
<br>
HTML/CSS<p>P태그</p>단락을 표현합니다.

* ul, li 태그: 계층구조(리스트)를 표현. ul은 순서없는 리스트
* strong, em: bold, italic
* blockquote: 인용구처럼?
<blockquote>명언을 여기에 적으면 있어보인다</blockquote>

* pre, code 태그: pre는 미리 지정된 서식, code: monospace? 글꼴로 표현
* a 태그: 하이퍼링크.
    * href: 이동할 주소
    * target: 놓침
* img 태그: 이미지를 넣어줌.
    * src
    * alt
* div 태그: 영역을 나누는 태그. display 속성이 block
    * 가장 많이 다루는 태그 중 하나

## Practice
### 메일 서비스 구성하기
---
https://github.com/seulgi0711/-html-css.git 다운받기

* Semantic Element: div와 같은 역할. 넷 다 같은건데, 가독성을 위해 이용
    * div
    * header
    * section: 하나의 카테고리 느낌..?
    * footer: 연락처, 작성자 등 문서 정보
    * 그 외 main, nav, aside, figure, article 등
    - 각자가 의미를 가지고 있어서 검색엔진 등 사용하기도 좋음 
    
    * 어디에 뭘 두는지는 각자의 취향인데, 대개 국룰이 어느정도 있긴 함

## CSS 기본 구조와 셀렉터
### Practice: 색 채워넣기
---
* CSS = Cascading Style Sheet. 문서의 표현을 기술하는 스일시트 언어

#### 구성요소
* 셀렉터, 속성, 값
```셀렉터 {
    속성: 값;
}
```
구조임
* Inline 방식은 각 태그마다 스타일을 다 적는 방식
* Embedded 방식은 head 안에 style로 감싸서 넣음. CSS가 간단할 때 많이 사용
* External 방식은 CSS를 별도의 파일로 분리하고 html에서 import. 가장 일반적인 방법
* CSS 상속: 부모 엘리먼트에 적용된 속성은 자식 엘리먼트에도 전해짐. 
---
#### 셀렉터
- ID 셀렉터
    - CSS를 적용할 요소를 지칭하는데, 아이디는 단 하나로 unique해야 함. id값으로 정하고, class는 id와 비슷하나 unique할 필요는 없음.
```<p id='hello'>안녕하세요.</p>
#hello{
    color: green;
}
```
- 클래스 셀렉터
가장 많이 씀
```
.hello{
    DO_Something
}
```
- 속성 셀렉터?
놓침

- 자손 셀렉터: 부모/자손 선택시 공백 사용
```
header h1,
header h2 {
    color: red;
}
```
- 부모와 (바로 밑?) 자식 선택 시 꺽쇠 사용
```
header > p {
    color: red;
}
```
 - 형제 셀렉터
 ```
 h1 + p {
     do_something
 }
 ```

* 바로 뒤 셀렉터
* 물결이면 뒤 모두
* *이면 전부

* 유사 클래스 셀렉터: header > p:hover, 
    * not: .box:not(:first-child) => first만 빼고 다

* 유사 엘리먼트 셀렉터: 주로 content와 사용, 주로 ::로 사용.
    * first-letter, first-line, selection 등이 있음
    * div같은거 앞/뒤에 margin, content같은거 줄 수 있음.
    * 블록 지정이 안 됨

### CSS 우선순위
---
* 여러 셀렉터로 선택되면 우선순위가 높은 걸로 적용. 같으면 뒤에 선언된 애가 덮어씌움
- 클래스 > 태그
- inline: 1000, ID: 100, 클래스/속성/유사 클래스: 10, 태그/유사 엘리먼트: 1 이고 이걸 포함된 만큼 점수를 더해서 우선순위 계산

### CSS 박스 모델 - 구성요소
* Margin: 바깥 여백
* Border: 테두리
* Padding: 패딩
* Content?

#### background-clip
* border-box: 테두리 영역까지
* padding-box: 안쪽 여백부터
* content-box: 내용 부분만
* text: 텍스트 위로만

#### box-sizing
* content-box: 내용을 기준으로 엘리먼트 크기를 잡음
* border-box: 테두리부터 기준으로 엘리먼트 크기를 잡음

#### width, height
* 엘리먼트의 크기를 지정
* max-width: 300px 이래놓으면 %로 아무리 줘도 못넘어감
* auto: content 내용으로 맞춤
* min-width, max-width로 auto랑 같이 사용 가능

#### margin
* margin: 10px => 네 면 모두 설정
* margin: 10px 0px => 위 아래
* margin: (3개) -> 위 옆 아래
* margin: (4개) -> 위부터 시계방향
* margin-left, right 등도 가능

#### padding
* margin이랑 비슷한데, 안쪽 여백이라는 게 특징
* padding을 주면 안쪽으로 padding이 들어옴

#### border
* 기본적으로 solid인데, border-style 값이 있어야 테두리가 생김?
* 테두리 색 변경
* 두께: border: 5px 이런식으로 변경가능
* 방향 설정 가능

### CSS display
---
엘리먼트를 블록과 인라인 속성으로 분류. 줄바꿈 유무 느낌인가
* 각 태그는 한 가지 display 속성을 가짐
* 예) p는 블록 속성, span은 인라인 

#### block
* 한 요소당 적어도 한 줄(block)
* 비슷한데, margin: 0 auto를 하면 가운데 정렬됨
#### inline
* 여러 요소가 한 행에 있을 수 있음, margin의 top,bottom 동작x
* text-align: center 등으로 가운데 정렬 가능
#### inline-block
* 일단 여러 요소가 한 행에 있을 수 있음
* margin, height, width 다 적용됨
* text-align도 가능
#### none
* 화면에서 보이지 않음
* 참고: visibility: hidden하고 비슷한데, 얘는 영역은 남겨줌

### CSS Flexible 박스
* 유연하게 조정한다는 뜻의 html5 기능?
* 성능면에서도 렌더링 시간이 예전 레이아웃 모델에 비해 빠름
* 기존과는 다르게 box가 아니라 container에 flex 속성을 줌
* 그럼 child들은 item으로 분류되고, 기본적으로 가로 방향(flex-direction: row)이며, 세로 방향(flex-direction: column)으로 할 수도 있음
* :nth-child(1) 이런 셀렉터가 존재
* flex 속성이 있는데, none이면 원래 지정된 공간을 차지하고, 숫자를 넣으면 각자 숫자만큼 비율로 나눠서 차지
* 

### Position
* absolute는 바로 위 엘리먼트
* fixed는 <strong>transform</strong> 속성을 가진 상위 엘리먼트가 기준

### Overflow
* 기본값은 그냥 보여줌
* hidden: 넘어가는 내용을 없앰
* scroll: 가로 세로 스크롤바를 항상 노출
* auto: 내용이 넘칠 때만 스크롤

### text-overlow
* 자동으로 말줄임을 해줌 (...)
* ellipsis와, overflow: hidden, white-space: nowrap 을 넣어줘야 함 nowrap은 글이 한 줄로만 나오게 해주는 역할
* z-index: 포지션이 static이 아닌 값일 때, 누가 더 위에 있을지를 결정함(정수?). 같은 거면 먼저 떠오른 애부터

## Font
* 서버 폰트를 사용하려면, font-face를 정의해주면 됨
* 링크태그나 css의 import를 이용해 받을 수도 있음
* 

* 이때부턴 불꽃 실습...