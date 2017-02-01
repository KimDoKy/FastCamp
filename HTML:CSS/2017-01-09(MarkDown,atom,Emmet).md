#	2017-01-09
-
###	MarkDown install & guide<br>
* [Download](http://macdown.uranusjr.com/)
* [Syntax Guide](https://guides.github.com/features/mastering-markdown/)

-
####	html first 실습<br>
- Mac : text edit - 포맷 - 일반 텍스트 만들기

-
###	atom install packages
* atom - 설정 ( command + , ) - install
	- emmet<br>
	- less-than-slash<br>

* atom 단축키
	- 단축키 검색 : shift + command + p
	- 줄 주석 : command + /
	- 여러 같은 태그 선택 : command + d

####	크롬 단축키
- 개발자 모드 : alt + command + i
- 새로고침 : command + shift + r




##	Emmet Document
- [Abbreviations Syntax Link](http://docs.emmet.io/abbreviations/syntax/)



#####	Nesting operators (중첩 연산자)

- Child: >
	- 하위 단계로 생성

	```
div>ul>li
	```
...will produce

	```
<div>
		<ul>
			<li></li>
		</ul>
</div>
	```
	
- Sibling: +
	- 동일 단계로 생성

	```
div+p+bq
	```
...will output

	```
<div></div>
<p></p>
<blockquote></blockquote>
	```

- Climb-up: ^
	- 상위 단계로 생성

	```
div+div>p>span+em^^^bq
	```
...will output to

	```
<div></div>
<div>
  	<p><span></span><em></em></p>
</div>
<blockquote></blockquote>
	```
	
- Multiplication: *
	- 출력되는 횟수를 정의

	```
ul>li*5
	```
	...outputs to

	```
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
	```
	
- Grouping: ()
	- 하위 트리를 그룹화

	```
div>(header>ul>li*2>a)+footer>p
	```
...expands to

	```
<div>
<header>
	<ul>
   		<li><a href=""></a></li>
   		<li><a href=""></a></li>
   </ul>
</header>
<footer>
   <p></p>
</footer>
</div>
	```

	```
(div>dl>(dt+dd)*3)+footer>p
	```
...produces

	```
<div>
    <dl>
        <dt></dt>
        <dd></dd>
        <dt></dt>
        <dd></dd>
        <dt></dt>
        <dd></dd>
    </dl>
</div>
<footer>
    <p></p>
</footer>
	```
-
#####	Attribute operators
출력된 요소의 속성을 수정하는데 사용


- ID and CLASS
	- id 와 class 속성을 지정

	```
div#header+div.page+div#footer.class1.class2.class3
	```
...will output

	```
<div id="header"></div>
<div class="page"></div>
<div id="footer" class="class1 class2 class3"></div>
	```

- Custom attributes (맞춤 속성)
	- [attr]으로 속성을 추가
	- 원하는 만큼 속성 배치 가능

	```
td[title="Hello world!" colspan=3]
	```
...outputs

	```
<td title="Hello world!" colspan="3"></td>
	```

- Item numbering: $ (품목 번호 지정)
	- * 연산자로 반복 생성시 $를 이용하여 속성내부값을 순차 출력함 

	```
ul>li.item$*5
	```
...outputs to

	```
<ul>
    <li class="item1"></li>
    <li class="item2"></li>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
	```  
	- $ 으로 0 자릿수 반복
	
	```
ul>li.item$$$*5
	```
...outputs to

	```
<ul>
    <li class="item001"></li>
    <li class="item002"></li>
    <li class="item003"></li>
    <li class="item004"></li>
    <li class="item005"></li>
</ul>
	```

- Changing numbering base and direction (번호 추가 및 방향 변경)
	- @- 으로 내림차순으로 변경

	```
ul>li.item$@-*5
	```
…outputs to

	```
<ul>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
    <li class="item2"></li>
    <li class="item1"></li>
</ul>
	```
	- @N 으로 카운터 기본값 변경
	
	```
ul>li.item$@3*5
	```
…transforms to

	```
<ul>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
    <li class="item6"></li>
    <li class="item7"></li>
</ul>
	```
	- 위 두가지를 함께 사용
	
	```
ul>li.item$@-3*5
	```
…is transformed to

	```
<ul>
    <li class="item7"></li>
    <li class="item6"></li>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
</ul>
	```

- Text: {}
	- 중괄호를 사용하여 요소에 텍스트 추가

	```
a{Click me}
	```
...will produce

	```
<a href="">Click me</a>
	```
	- 부모 컨텍스트를 주의해야한다.
	
	```
<!-- a{click}+b{here} -->
<a href="">click</a><b>here</b>
	```
	부호 하나로 다른 구조를 생성한다.
	
	```
<!-- a>{click}+b{here} -->
<a href="">click<b>here</b></a>
	```
	- 조금더 복잡한 예
	
	```
p>{Click }+a{here}+{ to continue}
	```
...produces

	```
<p>Click <a href="">here</a> to continue</p>
	```
	- 비교를 위해, 자식없이 작성 예
	
	```
p{Click }+a{here}+{ to continue}
	```
...produces

	```
<p>Click </p>
<a href="">here</a> to continue
	```

- Notes on abbreviation formatting (참고사항)
	- Emmet 구문을 더 쉽게 읽을 수 있는 형식을 사용하는게 좋다. 요소와 연산자 사이에 공백을 추가함으로써 쉽게 읽을 수 있다.

	```
(header > ul.nav > li*5) + footer
	```
	- 복잡한 약어는 쓸 필요가 없음. 오히려 오류를 발생 할 수 있음.











