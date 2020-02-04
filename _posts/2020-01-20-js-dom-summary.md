---
title: "[교육후기]Javascript와 함께하는 DOM 기초"
categories:
  - review
tags:
  - 교육
  - FE
---

# Javascript와 함께 하는 DOM 기초 for ROOKIES(v2020)

## 강의목표
<strong>Todolist 만들기!</strong>

* spec
  - TODO 추가, 완료할 수 있다.
  - TODO 완료/미완료 디자인 적용
  - 종류에 따라 갯수 카운팅
  - TODO 삭제 가능
  - 종류별로 보기

* 기본 환경 구성
  - 결국 Browser

### 자바스크립트의 사용
사용법은 크게 2가지. 
- \<script> 안에 src="path/to/jscode.js 를 넣음
- \<script> 안에 js 코드를 넣음

TIP) 이 교육에서는 html, js 파일 하나씩 가지고 실습을 진행함

#### HTML 내 script의 위치
HTML은 인터프레터처럼 작동해서, script가 헤더 등 참조할 엘리먼트보다 앞에 있으면 에러뜸. 해결법으로는
- script를 body의 끝에 두기
- head인데 onload를 포함하게 하기

### DOM 전체에서 엘리먼트 찾기
* getElementById: 해당 ID를 가지는 element
* getElementsByTagName: 해당 태그를 가지는 element의 array-like object
* getElementsByClassName: 해당 class를 가지는 element의 array-like object
* Live한 컬렉션이어서, 동적으로 결과가 반영됨
* Tree 구조여서, hierarchical한 접근 가능. 즉, 아래 코드처럼 사용 가능
```
element.getElementById(...).getElementsByTagName(...).getElementsByTagName(...) ...
```

### 엘리먼트 이벤트
* Event 추가 가능: onclick, addEventListener("click", function() ...) 등등
* remove 시에는 이벤트 함수의 참조를 넣어줘야 함

### 엘리먼트에서 값 가져오기/바꾸기
* input type="text"면 텍스트 입력창이 생김
* input.value, input.tpe, input.focus, input.blur 등 이것저것 사용가능

### 엘리먼트에 자식 엘리먼트 추가하기
```
var newP = document.createElement("p");
var newText = document.createTextNode("Hello, World!");

newP.appendChild(newText);
document.body.appendChild(newP); // body에 추가
alert('Then, we close');
document.body.removeChild(newP); // body에서 지워짐
```
이런 식으로 추가하면 되는데 Div, anchor 등 다양한 걸 만들어서 tree구조를 만들어서 붙일 수도 있음

또, 
```
element.innerHTML = '<div>Text</div>';
```
이런 식으로 직접 html을 편집할 수도 있음. C로 실행되기에 js보다 빠르기도 하고 편한 경우도 많음<br> 하지만, DOM을 아예 새로 만드는거라 작은 변화일 땐 비효율적
* 주의점: div.innerHTML += '텍스트'  이런 방식은 실행 시마다 html을 새로 만들어서 매우 비효율적임. 이런 코드가 많아지면 GC(Garbage Collector)가 막 돌아가는 걸 확인할 수 있음

### 체크박스 다루기
간단하니 생략

### 멍때리다가 뭔가 많이 건너뜀
## CSS 제어
element 내부의 style 어트리뷰트를 이용해 CSS를 적용할 수 있음

```
element.style.width = '30px';
element.style.display = 'flex';
element.style.border = '1px solid red';
```
예전에는 style 객체를 뺀 뒤 다 입력하고 다시 넣는 식으로 caching을 했어야 했다고 하는데 요즘은 큰 상관 없다고 하심

* JS 코드에서 아이디, 클래스 등 직접 적용 가능하고, class의 경우는 공백으로 구분.
```
element.className = 'myClass1 myClass2';
```

#### 클래스 지정
element의 class를 동적으로 지정할 수 있음
```
element.className = 'myClass';
```
띄어쓰기로 구분 가능. CSS랑 거의 비슷
* JS에서는 CSS랑 다르게, 특정 속성을 가진 클래스를 미리 만들고 엘리먼트를 특정 클래스로 만들었다 없앴다 하는 식으로 만듬

### querySelector
* LIVE 객체이며, CSS selector로 선택할 수 있는 엘리먼트들의 object를 가져옴
* querySelector, querySelectorAll이 있는데, 전자는 처음 걸린 하나만, 후자는 다 가져옴
* LIVE객체의 값을 받아온 뒤 업데이트해서 동적으로 변환 가능

### 엘리먼트 노드
* 기본적으로 DOM이 트리 구조라서, sibling이나 parent 등 다양한 객체 접근 가능.
* 자료구조에 대한 지식이 있으면 어렵지 않음
```
element.firstChild
element.lastChild
element.previousSibling
```
등등 다양