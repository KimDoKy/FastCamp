#	2017-01-10
-
###	html

작업 경로 : user/---/projects/html/

####Terminal 단축키  
`pwd` : 현재 경로 확인  
`open .` : 현재  위치  GUI로 열기
아톰 새창  
`command + shift + n`: 새창띄우기  
`command + \` : 폴더구조 보이기/가리기  

`shift + command + p` : auto indent

-
###	CSS
- 작업경로 : 
user/---/projects/css/

##### Block, Inline
- Block (블럭 요소)  
줄 바꿈이 일어나는 형태
기본적으로 width가 전체 너비의 값을 가진다.

- Inline (인라인 요소)  
줄 바꿈이 일어나지 않는 형태
기본적으로 자신의 내용만큼의 가로너비를 가진다.

- div, span (레이아웃 요소)  
오직 Block과 Inline방식의 레이아웃을 구현하는데에 사용

## 개발자 도구
단축키 `cmd + option + i`

요소(Elements)  
현재 랜더링 된 HTML요소들을 보여줌.

요소 선택 `cmd + chift + c`  
콘솔(Console)  
실시간으로 내부 자바스크립트를 테스트하거나, 로그를 출력함.

##	HTML Tag
##### Text tag
headline

```
<h1>HTML</h1>
<h2>역사</h2>
<h3>개발</h3>
<h4>최초규격</h4>
```

중요도 순으로 개요를 나타낼 때 사용.
학술문서나 검색엔진에서 검색시 중요하게 사용됨.
실제 글자크기등은 CSS에서 만들고자 하는 웹 페이지에 맞춰서 새로 설정하므로, 단계별로 구분할 제목이 있다면 hn태그를 사용하는 것이 좋다.

Line bleaks (줄 바꾸기)  
p태그 (Paragraph,문단)

```
<p>Lorem ipsum dolor sit amet.</p>
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Excepturi porro, magnam atque, recusandae ad dicta sit! Fugit, assumenda numquam explicabo!</p>
```
br태그 (Linebreak, 줄 바꾸기)

```
Lorem ipsum dolor sit amet.<br>
```
그 외
hr태그 (Horizontal rule,수평선)  
`<hr>`

blockquote태그 (인용문)  
```
<blockquote>인용문 내용</blockquote>
```

pre태그 (Preformatted text, 이미 형식화 된 텍스트)

```
<pre>
def pretag_test():
	val = 'pretag'
</pre>
```

###줄바꿈이 없는 텍스트 태그

####strong,b태그 (강조, 굴게 표시)

```
<strong>강조할 텍스트</strong>
<b>굴게 표시할 텍스트</b>
```

####em, i태그 (문맥상 특정부분 강조, 이탤릭 표시)

```
<em>강조할 텍스트</em>
<b>굴게 표시할 텍스트</b>
```

####mark태그 (형광펜 효과)
```
<mark>형광펜으로 그은 효과</mark>
```

#### Image, Hyperlink Tags
####링크(Anchor)
####a
```
<a href="http://www.daum.net" target="_blank" title="다음열기">daum</a>
```

- href : 이동할 페이지 주소
- target : 링크 걸린 페이지를 여는 방법(_self,_blank)
> _self : 링크를 클릭한 창의 주소를 바꿈<br>
> _blank : 새 창에서 링크를 열어줌<br>

- title : 마우스를 올렸을 때 보여줄 제목

####Image
####img

```
<img src="이미지경로" width="100" height="200" alt="이미지설명>
```
- src : 이미지 경로
- width,height : 가로/세로 크기(px단위)
- alt : 대체 텍스트 

####Data tags

#####List  
* Ordered List

```
<ol>
	<li>항목</li>
	<li>항목</li>
	<li>항목</li>
</ol>
```
* Unordered List

```
<ul>
	<li>항목</li>
	<li>항목</li>
	<li>항목</li>
</ul>
```
> 목록 형태로 나타나는 요소들은 ol,ul태그를 사용하여 구현 후, CSS로 제작하고자 하는 디자인에 맞게 스타일을 지정

* 목록 속성

```
<ol type="A" start="3" reversed>
	<li>First</li>
	<li>Second</li>
	<li>Third</li>
</ol>
```

- type

    값 | 설명
---|---
1 | 숫자(기본값)
a | 영문 소문자
A | 영문 대문자
i | 로마숫자 소문자
I | 로마숫자 대문자

- start : 시작할 숫자 지정
- reversed : 역순으로 표시

* Description List(정의 목록)

```
<dl>
  <dt>HTML</dt>
  <dd>Hyper Test Markup Language</dd>
  <dd>웹 페이지를 구현하는 마크업 언어</dd>

  <dd>CSS</dd>
  <dd>Cascading Style Sheet</dd>
  <dd>HTML의 형태를 저장하는 언어</dd>
</dl>
```
- dl : 정의 목록 태그<br>
- dt : 목록 중 개념을 나타냄<br>
- dd : 해당 개념의 정의를 나타냄<br>

> 목록과 정의 목록은 서로 중첩해서 사용이 가능

####table (테이블 요소)
#####테이블의 기본 구소

```
<table>
  <tr>
    <td>이름</td>
    <td>나이</td>
    <td>점수</td>
  </tr>
  <tr>
    <td>철수</td>
    <td>23세</td>
    <td>70점</td>
  </tr>
  <tr>
    <td>영희</td>
    <td>21세</td>
    <td>89점</td>
  </tr>
</table>
```
- table : 테이블의 시작<br>
- tr : 행<br>
- th : 테이블의 헤더 셀<br>
- td : 일반 셀

#####colspan, rowspan (쉘 병합)

```
<table>
  <tr>
    <td rowspan="3">a</td>
    <td>b</td>
  <tr>
    <td>c</td>
    <td>d</td>
    <td>e</td>
  </tr>
  <tr>
    <td colspan="2">c</td>
    <td>g</td>
  </tr>
</table>
```
- colspan : 가로로 셀을 병합
- rowspan : 세로로 셀을 병합

##### colgroup (하나 이상의 열을 그룹지음)

```
<table>
  <caption></caption>
  <colgroup>
    <col />
    <col />
    <col />
  </colgroup>
  <!-- 또는 -->
  <colgroup span="3"></colgroup>
</table>
```
```
table > colgroup {
	background: #f3f3f3;
	border-right: 3px double #333;
}
```

>colgroup을 사용하면, 특정 '열'또는, 특정 열의 '그룹'에 쉽게 속성을 줄 수 있음.

####행의 구조화

#####thead

```
<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>나이</th>
      <th>성별</th>
      <th>성적</th>
    </tr>
  </thead>
</table>
```

>colgroup이 열의 그룹화한다면, thead,tbody,tfoot은 행을 그룹화함.  
>이중 thead는 열의 제목을 나타냄.

#####tbody

```
<table>
  <tbody>
    <tr>
      <th>홍길동</th>
      <th>22세</th>
      <th>남자</th>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <th>김춘향</th>
      <th>19세</th>
      <th>여자</th>
    </tr>
  </tbody>
</table>
```
>tbody는 본문에 해당하는 영역.  
thead와 tfoot은 table에서 한 번만 선언될 수 있으나, tbody는 여러번 선언되어 행을 그룹화 가능.

#####tfoot

```
<table>
  <tfoot>
    <tr>
      <td colspan="3">평균</td>
      <td>87</td>
      <td></td>
    </tr>
  </tfoot>
</table>
```
>도표 하단을 나타냄.  
일반적으로 도표 전체의 합계나 결과를 표시하는 경우가 많음.

####Form 요소

#####form 태그

```
<form action="" method="get">
  <label for="username">ID</label>
  <input type="text" name="username">
</form>
```
>form은 브라우저에서 서버로 데이터를 전송하기 위한 태그

#####form - method 속성

method | -
-----|------
GET | POST
- GET : URL에 데이터를 담아 전달
- POST : URL과는 별도로 데이터를 전달

#####form action : form에서 데이터를 전송할 URL

####input 태그

```
<input type="text" id="username">
<input type="password" id="password">
<input type="radio" id="radio">
<input type="checkbox" id="checkbox">
<input type="button">
<input type="file" id="file">
<input type="submit">
<input type="reset">
<input type="hidden" id="hidden" value="hiddenValue">
```

* label 태그 (폼 요소에 레이블을 붙임)

* label내부에 표현

```
<label>ID<input type="text"></label>
```

* lavel과 별도로 표현

```
<label for="username">Username</label>
<input type="text" id="username">
```
>for속성과 form요소의 id를 연결시키면 label이 정확히 해당 form요소를 가리키게 됨


* select태그

```
<select name="number" id="select-id">
  <option value="1">First</option>
  <option value="2">Second</option>
  <option value="3">Third</option>
  <option value="4">Fourth</option>
</select>
```
> select태그는 여러개의 주어진 값 중 일부를 선택하는 역활

* multiple 속성

```
<select multiple="multiple">
  <option value="apple">apple</option>
  <option value="banana">banana</option>
  <option value="orange">orange</option>
</select>
```
>기본적으로 select요소는 option의 값을 한 개만 선택할 수 있음.  
multiple속성을 가질 경우, ctrl(cmd),shift키를 이용해 여러개의 값을 선택할 수 있음

* size 속성

```
<select size="2">
  <option value="apple">apple</option>
  <option value="banana">banana</option>
  <option value="orange">orange</option>
</select>
```
>한 번에 option을 몇 개 보여줄지 정함.

* optgroup 태그

```
<select size="2">
  <optgroup label="Fruits">
    <option value="apple">apple</option>
    <option value="banana">banana</option>
    <option value="orange">orange</option>
  </optgroup>
  <optgroup label="Colors">
    <option value="red">red</option>
    <option value="blue">blue</option>
    <option value="green">green</option>
  </optgroup>
</select>
```
>option을 그룹화하여 보여줌.

* button 태그

```
<button type="submit">submit</button>
<button type="reset">reset</button>
<button type="button">button</button>
```
>button요소는 input요소와 같은 type을 대체할 수 있음.

* fieldset, legend 태그

```
<fieldset>
  <legend>Login</legend>
  <label>username : <input type="text"></label>
  <label>password : <input type="password"></label>
</fieldset>
```
>legend요소는 fieldset의 첫번째 자식으로 사용해야함.  
fieldset은 다른 fieldset을 중첩해서 자식으로 가질 수 있음

####클래스와 아이디 속성  
* 네이밍 : 첫 글자는 알파벳으로 시작  
두 번째부터는 알파벳,숫자,-,_를 사용가능  
대소문자를 구분  
* 클래스와 아이디의 차이  
* id는 페이지에서 딱한번만 선언 가능.요소의 unique한 특성을 나타냄  
class는 여러번 사용 가능. 범용적인 부분을 나타냄.  
class와 id의 이름은 길더라도 요소의 의미에 적합하게 짓는 것이 좋다.  

```
<div class="chapter" id="chapter1">
  <h2>HTML</h2>
  <p>HTML강의를 시작해봅시다</p>
</div>

<div class="chapter" id="chapter2">
  <h2>CSS</h2>
  <p>Chapter2는 CSS입니다.</p>
</div>

<div class="chapter" id="chapter3">
  <h2>JavaScript</h2>
  <p>JavaScript는 배고파</p>
</div>
```
* 색상

```
<p style="color: #333;">Color #333</p>
<p style="color: DarkGreen;">Color DarkGreen</p>
```
>색상은 Hex code를 사용한 값과 HTML규격에 미리 정의된 ColorName을 사용할 수 있음

-
##	CSS
마크업 언어(HTML)가 실제 표시되는 방법을 기술하는 언어.
레이아웃과 스타일을 정의할 때 사용

문법

```
selector {
	property: value;
	}
```
```
#body-title {
  font-size: 14px;
  font-weight: bold;
  color: DarkSlateGrey;
}
```
###사용법  
1.내부 스타일 시트(Internal)

```
<head>
  <style type="text/css">
    #body-title {
      font-size: 14px;
      font-weight: bold;
    }
  </style>
</head>
```

2.인라인 스타일 시트(Inline)

```
<p id="body-title" style=:font-size:14px; font-weight: bold;>Internal</p>
```

3.외부 스타일 시트(External)

```
<head>s
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <p id="body-title">External</p>
</body>
```

###CSS선택자

Universal Selector (전체 선택자)

```
* {
  padding: 0;
  margin: 0;
}
```
HTML 페이지 내부의 모든 요소에 같은 CSS속성을 적용.
margin이나 padding값을 초기화하는 등, 기본값을 정할 때 주로 이용.
다만 모든 문서의 모든 요소를 읽기 때문에 페이지 로딩시간이 길어질수 있으니 자주 사용하는 것은 안좋음.

Tag Selector (태그 선택자)

```
h1 {
  color: red;
}
p {
  color: gray;
  margin-bottom: 10px;
}
```
해당하는 모든 HTML태그 요소를 저장

Class Selector (클래스 선택자)
HTML

```
<body>
  <div class="section">
    <p class="section-title">Lorem ipsum dolor sit amet.</p>
    <p class="section-content">Lorem ipsum dolor sit amet.</p>
  </div>
  <div class="section">
    <p class="section-title">Lorem ipsum dolor sit amet.</p>
    <p class="section-content">Lorem ipsum dolor sit amet.</p>
  </div>
</body>
```
CSS

```
.section {
  color: #333;
  margin-bottom: 40px;
}
p.section-title {
  font-size: 18px;
}
p.section-content {
  font-size: 13px;
  line-height: 13px;
  color: #999;
}
```
CSS에서는 마침표 기호로 나타내며, HTML에서는 주어진 값을 class속성값으로 가진 요소를 선택함.
마침표 앞에 태그를 붙여주면 범위는 지정한 태그에 한함.

ID Selector (ID 선택자)

HTML

```
<body>
  <h3 id="index-title">Lorem ipsum dolor sit amet.</h3>
  <p id="index-description">Lorem ipsum dolor sit amet.</p>
</body>
```
CSS

```
#index-title  {
  font-size: 18px;
}
p#index-description {
  font-size: 12px;
  color: #999;
}
```
CSS에서는 #기호로 나타내며, HTML에서는 주어진 값을 id속성값으로 가진 요소를 선택함.
HTML에서 id값은 오직 하나만 존재해야 함.
클래스선택자와 같이 앞에 TAG명을 입력할 수 있음.
ID선택자의 우선순위가 Class선택자의 우선순위보다 높으므로, 같은 속성에 서로 다른 값을 지정할 경우 ID선택자의 값이 적용됨.

Group Selector (그룹 선택자)

HTML

```
<body>
  <h3 id="index-title">Lorem ipsum dolor sit amet.</h3>
  <p id="index-description">Lorem ipsum dolor sit amet.</p>
</body>
```
CSS

```
#index-title  {
  font-size: 18px;
}
p#index-description {
  font-size: 12px;
  color: #999;
}
#index-title, #index-description {
  text-align: center;
}
```
여러 선택자에 같은 스타일을 적용하는 경우, 쉼표로 구분해 선택자를 나열해 사용.

Combinator Selector (복합 선택자)

- 하위선택자(descendant selector)

```
section ul {
  border: 1px solid black;
}
```

- 자식선택자(child selector)

```
section > ul {
  border: 1px solid black;
}
```
> 포함 관계를 가지는 태그들 사이에서는, 포함하는 요소는 '부모 요소', 포함되는 요소는 '자식 요소'라고 함.<br>
> 하위 선택자는 부모요소에 포함된 '모든'하위 요소를 지정하며, 자식 선택자는 부모요소의 '바로 아래; 자식 요소만을 지정함.

- 인접 형제 선택자(adjacent sibling)

```
h1 + ul {
  background: Azure;
  color: DarkBlue;
}
```

- 일반 형제 선택자(general sibling)

```
h1 ~ ul {
  background: Azure;
  color: DarkBlue;
}
```
같은 부모 요소를 가지는 요소들을 '형제 관계'라고 부름.
이때, 먼저 나오는 요소를 '형 요소', 나중에 나오는 요소는 '동생 요소'라 함.
(부모 요소 내부에서 보다 윗 줄에 쓰여진 것을 의미)
두 선택자 모두 형 요소에는 적용되지 않으며, 인접 형제 선택자는 조건을 충족하는 '첫번째'동생 요소만을 지정하며,
일반 형제 선택자는 조건을 충족하는 '모든' 동생 요소를 지정함.

- 속성 선택자(Attribute Selector)

패턴 | 의미 | 예제
----|----|----
E[attr]|'attr'속성이 포함된 요소E|`<E attr>Lorem</E>`
E[attr="val"]|'attr'속성의 값이 'val'인 요소E|`<E attr="val">Lorem</E>`
E[attr~="val"]|'attr'속성의 값에 'val'이 포함되는 요소 E<br>(공백으로 분리된 값이 일치해야 함)|`<E attr="val">Lorem</E>`<br>`<E attr="item val">Lorem</E>`<br><font color="red">`<E attr="value">Lorem</E>`</font>
E[attr&#124;="val"]|'attr'속성의 값에 'val'이 포함되거나 (공백분리),'val-'로 시작되는 요소E|`<E attr="val">Lorem</E>`<br>`<E attr="val-num3">Lorem</E>`<br><font color="red">`<E attr="value">Lorem</E>`</font>
E[attr^="val"]|'attr'속성의 값이 'val'로 시작하는 요소 E|`<E attr="val">Lorem</E>`<br>`<E attr="value">Lorem</E>`|
E[sttr$="val"]|'attr'속성의 값이 'val'로 끝나는 요소 E|`<E attr="val">Lorem</E>`<br>`<E attr="item-val">Lorem</E>`|
E[attr*="val"]|'attr'속성의 값에 'val'이 포함되는 요소 E<br>(공백이나 Dash(-)에 영향받지 않음)|`<E attr="val">Lorem</E>`<br>`<E attr=many itemval">Lorem</E>`<br>`<E attr="item-val">Lorem</E>`

- 가상 클래스 선택자(Pseudo-Classes Selector),가상 엘리먼트 선택자(Pseudo-Elements Selector)

패턴 | 의미
----|----
E:link|방문하지 않은 링크 E
E:visited|방문한 링크 E
E:active|E 요소에 마우스 클릭 또는 키보드 엔터가 눌린 동안
E:hover|E 요소에 마우스가 올라가 있는 동안
E:focus|E 요소에 포커스가 머물러 있는 동안
E::first-line|E 요소의 첫 번째 라인
E::first-letter|E 요소의 첫 번째 문자
E::before|E 요소의 시작 지점에 생성된 요소
E::after|E 요소의 끝 지점에 생성된 요소

##CSS Cascading(우선순위)

특정도(specify)값이 높은 순서대로 적용됨  

>특정도는 가장 구체적인 값을 의미함

####특정도 계산식

스타일 | 특정도
-----|-----
Inline (인라인 스타일) | A
ID Selector (ID 선택자) | B
Class Selector (클래스 선택자) | C
TAG Selector (태그 선택자) | D

>특정도는 CSS구문에서 해당하는 스타일의 수 * 특정도 값을 모두 더한 값  
>  
> 클래스 선택자가 3개 존재하며, 태그선택자가 있는 CSS구문이라면  
> 클래스선택자의 갯수(3) * 클래스선택자의 특저도(10) + 태그선탲가의 개수(1) * 태그 선택자의 특정도(1) = 31의 특정도를 가지게 됩니다.
> 
ex)

css 구문 | 특정도
----|----
p { color: gray; } | 1D
p:first-line { color: black; }|2D
.wrap ( color: black; }|1C
p.wrap ( color: black; }|1C,1D
p.wrap>.item { color: black; }|2C,1D
#wrap {color: black; } | 1B
p#wrap { color: black; }|1B,1D
<!-HTML-><br>`<p style="color: black;">Example</p>`|1A

####우선순위의 흑마법
인라인 스타일과 important

스타일 | 특정도
-----|-----
important|Absolute
Inline (인라인 스타일) | A
ID Selector (ID 선택자) | B
Class Selector (클래스 선택자) | C
TAG Selector (태그 선택자) | D

```css
p {
	font-size: 30px !important;
	color: green !important;
}
```
```
<p style="font-size: 10px; color: green;">important는 모든걸 무시하지</p>
```
-
### CSS Typography(서체)
#####color

```
h1 {
	color: #ff0000;
}
```
>color속성은 글자 색을 지정함  
>HEX코드,rgb(), rgba(), 색상명이 올 수 있음  

#####font family

```
body {
	font-family: "돋움", dotum, "굴림",gulim, arial, helvetica, sans-serif;
}
```
>웹 페이지를 방문한 사용자가 없을 경우를 대비해, 다양한 서체들은 선언  
>순서대로 해당 서체가 없을 경우 순차적으로 가장 먼저 찾은 폰트를 사용  

#####font type
serif : 궁서체, 바탕체, 명조체 등
monospace : 고정 폭. 프로그래밍 코드에 주로 사용
Sans-serif : 돋움, 굴림, 나눔 고딕 등
Cursive : 필기체. 가독성이 떨어짐. 디자인용  

#####font size

```
body {
	font-size: 14px;
}
h1 {
	font-size: 2em;
}
```
>em은 부모요소로부터의 비율  
>부모의 포트가 14px이면 2em은 28px  

#####font style

```
p {
	font-style: italic;
}
```
#####font weight

```
p {
	font-weight: bold;
}
p {
	font-weight: 700;
}
```
>lighter와 bolder는, 해당 서체의 Lighter 또는 Bolder서체가 있어야 표현되며, 해당 서체가 없을 경우 narmal, bold와 동일한 굵기로 나타남.

#####Line height
```
p {
	line-height: 1.5;
}
```
>font-size가 px로 고정되어 있다면, line-height에도 px을 사용할 수 있음.

#####text-decoration

```
p.item1 {
  text-decoration: none;
}
p.item2 {
  text-decoration: underline;
}
p.item3 {
  text-decoration: overline;
}
p.item4 {
  text-decoration: line-through;
}
```

- underline : 밑줄  
- overline : 윗줄  
- line-through : 취소선  

#####text-align  

```
p.item1 {
  text-align: left;
}
p.item2 {
  text-align: center;
}
p.item3 {
  text-align: right;
}
p.item4 {
  text-align: justify;
}
```
>justify는 우측 끝 부분을 깔끔하게 양쪽 정렬해주지만, 줄의 내부 간격이 뒤틀리므로 잘 사용하지 않음  

#####text-indent (들여쓰기)

```
p {
	text-indent: 1em;
}
```
#####text-transform (대소문자 변환)

```
p.item1 {
  text-transform: capitalize;
}
p.item2 {
  text-transform: uppercase;
}
p.item3 {
  text-transform: lowercase;
}
```
- capitalize : 첫글자만 대문자로  
- uppercase : 모두 대문자로 
- lowercase : 모두 소문자로

#####letter-spacing(자간)

```
p {
	letter-spacing: 5px;
}
```

#####word-spacing(단어 간격)

```
p {
	word-spacing: 5px;
}
```
#####vertical-align(요소간 수직 정렬)  
박스 내에서의 수직 정렬이 아닌, 나란히 오는 인라인 요소간의 정렬

#####text-align (줄 바꿈 설정)

```
p.item1 {
  white-space: nowrap;
}
p.item2 {
  white-space: pre;
}
p.item3 {
  white-space: pre-line;
}
p.item4 {
  white-space: pre-wrap;
}
```
- nowrap : 줄 바꿈이 없음
- pre : 줄 바꿈, 띄어쓰기, 공백 등 모든것이 보여지며,  
박스를 벗어나도 줄 바꿈이 일어나지 않음
- pre-line : 줄 바꿈만 보여주며, 띄어쓰기는 무시함.  
자동으로 줄바꿈이 됨.
- pre-wrap : 줄 바꿈과 띄어쓰기를 모두 보여주며 자동으로 줄 바꿈이 됨



### CSS Background(배경 스타일)
#####background-color
```
div {
  background-color: #eee;
  background-color: rgb(230, 222,120);
  background-color: rgba(230, 22, 120, 0.4);
}
```
#####background-image
```
div {
  background-image: url('../image/icon.png');
}
```
>이미지이 주소는 상대경로, 절대경로 모두 사용 가능  

####background-repeat
```
div {
  background-repeat: no-repeat;
  background-repeat: repeat;
  background-repeat: repeat-x;
  background-repeat: repeat-y;
}
```

- no-repeat : 반복하지 않음  
- repeat : 가로세로 바둑판 형식으로 반복  
- repeat-x : 가로(x축)으로만 반복
- repeat-y : 세로(y축)으로만 반복

####background-position
```
div {
  background-position: 50% 16px;
  background-position: center bottom;
}
```
>삽입됩 이미지의 좌표를 정해줌  
>두 개의 값을 받으며, 각각 x축,y축의 값을 가짐  

####background-attachment(배겨 이미지 고정)
```
div {
  background-attachment: local;
  background-attachment: scroll;
  background-attachment: fixed;
}
```
- local : 요소의 왼쪽 상단을 기준으로 이미지를 표현하며, 스크롤 시 이미지가 같이 움직임  
- scroll : 요소의 왼쪽 상단을 기준으로 이미지를 표현하며, 스크롤시 이미지가 함꼐 움직이지 않고 고정  
- fixed : 요소와 관계없이 웹페이지 전체화면을 기준으로 이미지가 표시되며, 스크롤시 함께 움직이지 않고 고정.(background-position좌표를 웹 페이지 화면 전체기준으로 함)

### CSS Border(테두리 스타일)
####Element's direction(요소의 방향)  
>순서는 시계방향으로 진행  
top-right-bottom-left 순으로 진행  

####border-width (선 굵기)
```
div {
  border-width: 3px;
  border-top-width: 4px;
  border-width: 3px 4px 5px 6px;
  border-width: 5px 10px;
}
```
- border-width: 3px;  
상하좌우 모두 3px  
- border-top-width: 4px;  
상단만 4px  
- border-width: 3px 4px 5px 6px;  
상하좌우 순서대로  
- border-width: 5px 10px;  
상하 5px, 좌우 10px

####border-style (선 형태)
```
div {
  border-style: solid;
  border-style: double;
  border-style: solid double dashed dotted;
}
```
- solid : 실선
- dotted : 점선
- dashed : 바느질선 형태의 점선
- double : 이중선
- groove : 입체적
- ridge : groove와 반대
- insert : 요소 전체가 안으로 들어가 보임
- outset : 요소 전체가 바깥으로 나와 보임

####border-color
```
div {
  border-color: #aaa;
  border-color: red blue green yellow;
}
```
-
### CSS Box Model
>CSS요소는 박스 형태를 이루며,  
>박스는 콘텐츠, 패딩, 보더, 마진의 4가지로 이루어짐. (개발자 도구에서 확인 가능)

####블록 요소와 인라인 요소의 차이
- **인라인 요소**는 **가로마진**만 가질 수 있음.  
인라인 요소는 길이/높이는 지정이 불가능함 (내용에 자동으로 맞추어짐)
- **블록 요소**는 **가로/세로 마진**을 모두 가짐

####margin (바깥 여백)
```
div {
  margin-top: 10px;
  margin-bottom: 30px;
  margin: 10px 0;
  margin: 40px 20px 30px 50px;
}
```
####margin collspse (여백 겹침)
```
<div style="margin-bottom:10px;">Box 1</div>
<div style="margin-top:30px;">Box 2</div>
```
>두 블록요소의 마진이 서로 겹칠 경우, 해당한 마진값이 더해지는것이 아니라 둘 중 큰 값만이 적용됨 (가로에서는 해당 현상 없음)  
>따라서 서로 위/아래로 겹치는 마진값을 준 경우, 한쪽에만 값을 몰아주거나, padding을 활용하는 방식으로 해결해야 함

####padding (내부 여백)
```
div {
  padding-top: 10px;
  padding-bottom: 10px;
  padding: 10px 0;
  padding: 10px 20px 30px 40px;
}
```
>padding과 margin을 구분하는 가장 쉬운법은,   padding과 margin을 준 요소에 background-color를 지정한 후 개발자 모드에서 해당 요소가 차지하는 공간을 확인하면 됨  

####width, height
```
div {
	width: 100px;
	height: 50px;
}
```
####box-sizing
```
div {
  width: 200px;
  height: 50px;
  padding: 10px;
  border: 1px solid black;
  margin: 15px;
  box-sizing: border-box;
}
```
>box-sizing에 border-box를 지정하면, 요소의 width값에 맞추어 padding, border값을 제외한 width가 새로 적용 됨.

### CSS List Style
####list-style-type (리스트 앞 Bullet타입 설정)
```
ul {
  list-style-type: disc;
  list-style-type: circle;
  list-style-type: square;

  list-style-type: decimal;
  list-style-type: lower-alpha;
  list-style-type: upper-alpha;
  list-style-type: lower-roman;
  list-style-type: upper-roman;
}
```
>disc,circle,square는 Unordered List에 어울리는 속성이며,  
>decimal,alpha,roman은 Ordered List에 어울리는 속성

####list-style-image (리스트 bullet에 이미지 지정)
```
ul {
	list-style-image: url('image/mic.png');
}
```
####list-style-position
```
ul {
	list-style-position: inside;
	list-style-position: outside;
}
```

### CSS Table Style
```html
<table>
  <tr>
    <td>A</td>
    <td>B</td>
    <td>C</td>
  </tr>
  <tr>
    <td>1</td>
    <td>2</td>
    <td>3</td>
  </tr>
  <tr>
    <td>4</td>
    <td>5</td>
    <td>6</td>
  </tr>
</table>
```
```
tr, td {
  border: 1px solid black;
  width: 30px;
  text-align: center;
}
```
####border-collapse (테두리합치기)
```
table {
	border-collapse: collapse;
}
tr, td {
  border: 1px solid black;
  width: 30px;
  text-align: center;
}
```
>테이블의 셀간 공백을 합쳐서 없애는 속성  
>해당 속성은 오직 table요소에서만 사용 가능  


#####border-spacing(테이블 셀 간격)
```
table {
	border-spacing: 10px;
}
tr, td {
  border: 1px solid black;
  width: 30px;
  text-align: center;
}
```
>셀 간 간격을 지정할 때는, border-collapsw가 'collapse'갑시면 적용되지 않음

####empty-cells (빈 셀의 표시)
```
table {
	border-spacing: 10px;
}
tr, td {
  border: 1px solid black;
  width: 30px;
  text-align: center;
  empty-cells: hidel
}
```
####table-layout
>테이블의 기본 설정은 내용이 긴 쪽이 더 늘어남  
>`table-layout: auto;`
>table-layout의 속성을 fixed로 설정하면, 셀 길이가 일정하게 유지됨  
>(이 때, table에는 width property가 설정되어있어야 함)
