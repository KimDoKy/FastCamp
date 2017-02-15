# Built-in template tags and filters
Django에 내장 된 템플릿 태그와 필터에 대한 내용입니다.

## Built-in tag reference
### autoescape
> 자동으로 특수 문자들을 변환시켜주는 개념
> 특수문자들은 HTML문서 에서 `&`,`>`, `<`, `"` , `'` 에 해당합니다.
> 이 문자들은 텍스트 이외에도 특별한 기능들을 수행합니다. (연산자, 태그 등)
`auto-escape` 동작을 제어합니다. `on / off` 의 인자를 가지며, `endautoescape` 종료 태그로 닫힙니다.
`auto-escape`가 실행 중일때 결과를 출력하기 전에 모든 가변 내용에 HTML escape가 적용됩니다.
이것은 각 변수들에 수동으로 escape filter를 적용하는 것과 같습니다.
escape filter가 적용되었거나 escape에서 안전함으로 표시가 되었다면 예외입니다.

```
{% autoescape on %}
    {{ body }}
{% endautoescape %}
```

-
### block
하위 템플릿으로 덮어 쓸 수 잇는 블록을 정의 합니다.
[Template inheritance 참조](https://docs.djangoproject.com/en/1.10/ref/templates/language/#template-inheritance)

-
### comment
`{% comment %}`와 `{% endcomment %}` 사이의 모든 것을 무시합니다.
이 사이에 기술한 내용은 코드가 사용되지 않기 때문에 주석으로 처리할때 사용합니다.

```
<p>Rendered text with {{ pub_date|date:"c" }}</p>
{% comment "Optional note" %}
    <p>Commented out text with {{ create_date|date:"c" }}</p>
{% endcomment %}
```
주석 태그는 중첩 될 수 없습니다.

-
### csrf_token(Cross Site Request Forgeries)
POST방식으로 요청을 주거나 받을때 보안을 위해 사용합니다.
[Cross Site Request Forgeries 참조](https://docs.djangoproject.com/en/1.10/ref/csrf/)

-
### cycle
**이 태그가 발견 될 때마다 인수 중 하나를 생성**합니다. 첫 번째 인수는 첫 번째 발생에서 생성되고 두 번째 인수는 두 번째 발생에서 생성됩니다. 모든 인수가 모두 소모되면 태그는 첫 번째 인수로 순환하고 다시 인수를 생성합니다.

이 태그는 **루프에서 특히 유용**합니다.

```
{% for o in some_list %}
    <tr class="{% cycle 'row1' 'row2' %}">
        ...
    </tr>
{% endfor %}

# 첫 번째 반복에서는 row1 클래스를 참조하는 HTML을 생성하고, 두 번째 행은 row2, 세 번째 행은 row1, 두 번째 행을 다시 한 번 반복하는 등 반복문을 반복합니다.
```
**변수도 사용**할 수 있습니다. 

```
# 예를 들어, 두 개의 템플릿 변수 인 rowvalue1과 rowvalue2가있는 경우 다음과 같이 해당 값을 번갈아 나타낼 수 있습니다.

{% for o in some_list %}
    <tr class="{% cycle rowvalue1 rowvalue2 %}">
        ...
    </tr>
{% endfor %}
```
```
# 사이클에 포함 된 변수가 이스케이프됩니다.
# 다음을 사용하여 자동 이스케이프를 비활성화 할 수 있습니다.

{% for o in some_list %}
    <tr class="{% autoescape off %}{% cycle rowvalue1 rowvalue2 %}{% endautoescape %}">
        ...
    </tr>
{% endfor %}
```
```
# 변수와 문자열을 혼합 할 수 있습니다.

{% for o in some_list %}
    <tr class="{% cycle 'row1' rowvalue2 'row3' %}">
        ...
    </tr>
{% endfor %}
```
어떤 경우에는 다음 값으로 진행하지 않고 사이클의 현재 값을 참조 할 수 있습니다. 이렇게하려면 다음과 같이 `{% cycle %}` 태그에 "as"를 사용하여 이름을 지정하면됩니다.

```
{% cycle 'row1' 'row2' as rowcolors %}
```
그런 다음 cycle 이름을 컨텍스트 변수로 참조하여 템플릿에 원하는 모든 사이클의 현재 값을 삽입 할 수 있습니다. 원래 싸이클 태그와 독립적으로 싸이클을 다음 값으로 이동하려면 다른 싸이클 태그를 사용하고 변수의 이름을 지정할 수 있습니다.

```
# 원래 싸이클 태그와 독립적으로 싸이클을 다음 값으로 이동하려면 다른 싸이클 태그를 사용하고 변수의 이름을 지정할 수 있습니다.

<tr>
    <td class="{% cycle 'row1' 'row2' as rowcolors %}">...</td>
    <td class="{{ rowcolors }}">...</td>
</tr>
<tr>
    <td class="{% cycle rowcolors %}">...</td>
    <td class="{{ rowcolors }}">...</td>
</tr>
```
아래와 같이 출력합니다.

```
<tr>
    <td class="row1">...</td>
    <td class="row1">...</td>
</tr>
<tr>
    <td class="row2">...</td>
    <td class="row2">...</td>
</tr>
```
싸이클 태그에 공백으로 구분 된 여러 개의 값을 사용할 수 있습니다. 작은 따옴표 ( ') 나 큰 따옴표 ( ")로 묶인 값은 문자열 리터럴로 취급되며 따옴표가없는 값은 템플릿 변수로 취급됩니다.

기본적으로 사이클 태그에 as 키워드를 사용하면 cycle을 시작하는 `{% cycle %}`의 사용법 자체가 cycle의 첫 번째 값을 생성합니다. 중첩 루프 또는 포함 된 템플릿에서 값을 사용하려는 경우 문제가 될 수 있습니다. 주기를 선언하고 첫 번째 값만 생성하지 않으려는 경우 자동 키워드를 태그의 마지막 키워드로 추가 할 수 있습니다.

```
{% for obj in some_list %}
    {% cycle 'row1' 'row2' as rowcolors silent %}
    <tr class="{{ rowcolors }}">{% include "subtemplate.html" %}</tr>
{% endfor %}
```
그러면 row1과 row2가 교대로 반복되는 `<tr>` 요소의 목록이 출력됩니다. 하위 템플릿은 컨텍스트에서 rowcolors에 액세스 할 수 있으며 값은이를 둘러싸는 `<tr>` 클래스와 일치합니다. 자동 키워드를 생략 할 경우 row1과 row2는 `<tr>` 요소 외부의 일반 텍스트로 방출됩니다.
silent 키워드가 싸이클 정의에 사용될 때, auto-cycle은 해당 싸이클 태그의 모든 후속 용도에 자동으로 적용됩니다. {% cycle %}에 대한 두 번째 호출이 자동을 지정하지 않더라도 다음 템플릿은 아무 것도 출력하지 않습니다.

```
{% cycle 'row1' 'row2' as rowcolors silent %}
{% cycle rowcolors %}
```

-
### debug
현재 컨텍스트 및 가져온 모듈을 포함하여 전체로드의 디버깅 정보를 출력합니다.

-
### extends
이 템플릿이 **상위 템플릿을 확장**한다는 신호입니다.
이 태그는 두 가지 방법으로 사용할 수 있습니다.
- `{% extends "base.html"%}(따옴표 포함)`은 **리터럴 값 "base.html"을 확장 할 상위 템플릿의 이름으로 사용**합니다.
- `{% extends variable %}`는 variable의 값을 사용합니다. 변수가 문자열로 평가되면 Django는 **그 문자열을 부모 템플릿의 이름으로 사용**합니다. 변수가 Template 객체로 평가되면 Django는 그 객체를 부모 템플릿으로 사용합니다.

문자열 인수는 상대경로(`./` , `../`)일 수 있습니다.

```
{% extends "./base2.html" %}
{% extends "../base1.html" %}
{% extends "./my/base3.html" %}
```

-
### filter
하나 이상의 filter를 통해 블록의 내용을 필터링합니다.  
여러개의 filter를 파이프와 함께 지정할 수 있고, filter 변수 구문처럼 인수를 가질 수 있습니다.

block에는 filter와 endfilter 태그 사이의 모든 텍스트가 모함됩니다.

```
{% filter force_escape|lower %}
    This text will be HTML-escaped, and will appear in all lowercase.
{% endfilter %}
```
>**Note**  
`escape` 및 `safe filter`는 허용되는 인수가 아닙니다. 대신 `autoescape` 태그를 사용하여 템플릿 코드 블록에 대한 `auto-escape`를 관리하십시오.

-
### firstof
False가 아닌 첫 번째 인수 변수를 출력합니다. 전달 된 모든 변수가 False이면 아무 것도 출력하지 않습니다.

```
{% firstof var1 var2 var3 %}
```
아래와 같은 의미입니다.

```
{% if var1 %}
    {{ var1 }}
{% elif var2 %}
    {{ var2 }}
{% elif var3 %}
    {{ var3 }}
{% endif %}
```
전달 된 **모든 변수가 False** 인 경우 **리터럴 문자열을 대체 값으로 사용**할 수도 있습니다.

```
{% firstof var1 var2 var3 "fallback value" %}

# 이 태그는 변수 값을 자동으로 escape합니다.
```
```
# 자동 이스케이프를 비활성화 할 수 있습니다.

{% autoescape off %}
    {% firstof var1 var2 var3 "<strong>fallback value</strong>" %}
{% endautoescape %}
```
또는 일부 변수 만 escape 해야하는 경우 다음을 사용할 수 있습니다.

```
{% firstof var1 var2|safe var3 "<strong>fallback value</strong>"|safe %}
```
`{% firstof var1 var2 var3 value}}` 구문을 사용하여 **출력을 변수에 저장**할 수 있습니다.

-
### for
배열의 각 항목을 **루프**하여 항목을 컨텍스트 변수에서 사용할 수 있도록합니다.

```
# athlete_list에 제공된 선수 목록을 표시

<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>

# {% for obj in list reversed %} 를 이용하여 역순으로 사용할 수 있습니다.
```
목록안의 목록을 반복해야 해야 한다면, 각 하위 목록의 값을 개별 변수로 사용할 수 있습니다.

```
{% for x, y in points %}
    There is a point at {{ x }},{{ y }}
{% endfor %}
```

dict의 항목에 엑서스해야하는 경우에도 사용이 가능합니다.

```
{% for key, value in data.items %}
    {{ key }}: {{ value }}
{% endfor %}
```
점(.) 연산자의 경우 사전 조회는 메소드 조회보다 우선합니다.
따라서 dict에 'items'라는 키가 포함되어 있으면 `data.items`는 `data.items()` 대신 `[items]`를 반환합니다. 템플릿 (항목, 값, 키 등)에서 이러한 메서드를 사용하려면 dict 메서드와 같은 이름의 키를 추가하지 마십시오.

for 루프는 루프 내에서 사용할 수있는 여러 변수를 설정합니다.

Variable | Description
---|---
forloop.counter | 루프의 현재 반복 (1-indexed)
forloop.counter0 | 루프의 현재 반복 (0-indexed)
forloop.revcounter | 루프의 끝에서 반복 횟수 (1-indexed)
forloop.revcounter0 | 루프의 끝에서 반복 횟수 (0-indexed)
forloop.first | 처음으로 루프를 통과하면 True
forloop.last | 마지막으로 루프를 통과하면 True
forloop.parentloop | 중첩 루프의 경우 바깥루프

-
### for ... empty
`for` 태그는 **지정된 배열이 비어 있거나 찾을 수없는 경우** 텍스트가 표시되는 `{% empty %}`을 사용할 수 있습니다.

```
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% empty %}
    <li>Sorry, no athletes in this list.</li>
{% endfor %}
</ul>
```
위의 코드는 아래와 같지만, 더 짧고 처리가 더 빠릅니다.

```
<ul>
  {% if athlete_list %}
    {% for athlete in athlete_list %}
      <li>{{ athlete.name }}</li>
    {% endfor %}
  {% else %}
    <li>Sorry, no athletes in this list.</li>
  {% endif %}
</ul>
```

-
### if
`{% if %}` 태그는 변수를 평가하고 해당 변수가 "`True`"(즉, 존재하고 비어 있지 않으며 False가 아닌 경우) block 의 내용을 출력합니다.

```
{% if athlete_list %}
    Number of athletes: {{ athlete_list|length }}
{% elif athlete_in_locker_room_list %}
    Athletes should be out of the locker room soon!
{% else %}
    No athletes.
{% endif %}
```
위의 경우 `athlete_list`가 비어 있지 않으면, `Number of athletes: {{ athlete_list|length }}`는 `{{athlete_list | length}}` 변수로 표시됩니다.

`if` 태그는 하나 이상의 `{% elif %}` 뿐 아니라 이전 조건이 모두 실패한 경우 표시 될 `{% else %}`을 사용할 수 있습니다. 선택 사항입니다.

#### Boolean operators
태그가 `and`, `or`또는 `not`을 사용하여 **여러 변수를 테스트**하거나 **주어진 변수를 무효화** 할 수있는 경우

```
{% if athlete_list and coach_list %}
    Both athletes and coaches are available.
{% endif %}

{% if not athlete_list %}
    There are no athletes.
{% endif %}

{% if athlete_list or coach_list %}
    There are some athletes or some coaches.
{% endif %}

{% if not athlete_list or coach_list %}
    There are no athletes or there are some coaches.
{% endif %}

{% if athlete_list and not coach_list %}
    There are some athletes and absolutely no coaches.
{% endif %}
```

동일한 태그 내에서 `and`, `or`을 모두 사용 할 수 있으며, 위의 예보다 아래의 예가 우선순위가 높습니다.

```
{% if athlete_list and coach_list or cheerleader_list %}
```
아래와 같이 해석할 수 있습니다.

```
if (athlete_list and coach_list) or cheerleader_list
```
`if` 태크에서 실제 괄호 `( )` 사용은 문법에 어긋납니다. 우선순위를 나타내려면 중첩된 `if` 태그를 사용해야 합니다.

`if`태그는 `==`, `!=`, `<`, `>`, `<=`, `>=`, `in`, `not in`, `is`, `is not` 연산자를 사용할 수 있습니다.

##### == operator
일치

```
{% if somevar == "x" %}
  This appears if variable somevar equals the string "x"
{% endif %}
```

-
##### != operator
불일치

```
{% if somevar != "x" %}
  This appears if variable somevar does not equal the string "x",
  or if somevar is not found in the context
{% endif %}
```

-
##### < operator
..보다 미만

```
{% if somevar < 100 %}
  This appears if variable somevar is less than 100.
{% endif %}
```
-

##### > operator
..보다 이상

```
{% if somevar > 0 %}
  This appears if variable somevar is greater than 0.
{% endif %}
```

-
##### <= operator
..보다 작거나 같다

```
{% if somevar <= 100 %}
  This appears if variable somevar is less than 100 or equal to 100.
{% endif %}
```

-
##### >= operator
..보다 크거나 같다

```
{% if somevar >= 1 %}
  This appears if variable somevar is greater than 1 or equal to 1.
{% endif %}
```

-
##### in operator
원하는 값이 리스트 안에 들어 있는지 테스트 합니다.
x에서 y를 해석하는 방법의 몇 가지 예입니다.

```
{% if "bc" in "abcdef" %}
  This appears since "bc" is a substring of "abcdef"
{% endif %}

{% if "hello" in greetings %}
  If greetings is a list or set, one element of which is the string
  "hello", this will appear.
{% endif %}

{% if user in users %}
  If users is a QuerySet, this will appear if user is an
  instance that belongs to the QuerySet.
{% endif %}
```

-
##### not in operator
`in` 연산자와 반대입니다.

-
##### is operator
2개의 변수가 같은지 식별합니다.

```
{% if somevar is True %}
  This appears if and only if somevar is True.
{% endif %}

{% if somevar is None %}
  This appears if somevar is None, or if somevar is not found in the context.
{% endif %}
```

-
##### is not operator
`is` 연산자와 반대입니다.

```
{% if somevar is not True %}
  This appears if somevar is not True, or if somevar is not found in the
  context.
{% endif %}

{% if somevar is not None %}
  This appears if and only if somevar is not None.
{% endif %}
```

-
#### Filters
`if`에서 `filter`를 사용할 수 있습니다.

```
{% if messages|length >= 100 %}
   You have lots of messages today!
{% endif %}
```

-
#### Complex expressions
위의 모든 연산자를 결합하여 복잡한 표현식을 구성 할 수 있습니다. 그러한 표현식의 경우 표현식이 평가 될 때 연산자가 그룹화되는 방식, 즉 선행 규칙을 알아야합니다. 연산자의 우선 순위는 가장 낮은 것에서 가장 높은 것까지 다음과 같습니다.

`or`, `and`, `not`, `in`, `==`, `!=`, `<`, `>`, `<=`, `>=`

```
{% if a == b or c == d and e %}

# 위 코드는 아래와 같이 해석된다.

(a == b) or ((c == d) and e)
```
다른 우선순위가 필요한 경우 중첩 if문을 사용하는게 맞지만, 우선 순위 규칙을 모르는 사람들을 위래 이렇게 표현하는게 더 나을수도 있습니다.

비교연산자는 파이썬이나 수학 쵸기법처럼 연쇄적으로 사용할 수 없습니다.

```
# 사용 불가
{% if a > b > c %}  (WRONG)

# 사용 가능
{% if a > b and b > c %}
```

-
### ifequal and ifnotequal
`{% if a b %} ... {% endif %}`는 `{% if a == b %} ... {% endif %}`를 쓰는 쓸모없는 방법입니다. 마찬가지로 `{% ifnotequal a b %} ... {% endifnotequal %}`는 `{% if a! = b %} ... {% endif %}`로 대체됩니다. `ifequal` 및 `ifnotequal` 태그는 향후 릴리스에서 더 이상 사용되지 않습니다.

-
### ifchanged
값이 루프의 마지막 반복에서 변경되었는지 확인합니다.
`{% ifchanged %}` 블록 태그는 루프 내에서 사용됩니다. 두 가지 용도가 있습니다.

1. 렌더링 된 내용을 이전 상태와 비교하여 내용이 변경된 경우에만 내용을 표시합니다. 예를 들어 일 목록 만 표시되며 변경된 경우 해당 월 만 표시됩니다.

```
<h1>Archive for {{ year }}</h1>

{% for date in days %}
    {% ifchanged %}<h3>{{ date|date:"F" }}</h3>{% endifchanged %}
    <a href="{{ date|date:"M/d"|lower }}/">{{ date|date:"j" }}</a>
{% endfor %}
```
2. 하나 이상의 변수가 주어지면 변수가 변경되었는지 확인합니다. 예를 들어, 다음은 변경 될 때마다 날짜를 표시하고 시간 또는 날짜가 변경된 경우 시간을 표시합니다.

```
{% for date in days %}
    {% ifchanged date.date %} {{ date.date }} {% endifchanged %}
    {% ifchanged date.hour date.date %}
        {{ date.hour }}
    {% endifchanged %}
{% endfor %}
```
`ifchanged` 태그는 값이 변경되지 않은 경우 표시 될 선택적 `{% else %}` 을 사용할 수도 있습니다.

```
{% for match in matches %}
    <div style="background-color:
        {% ifchanged match.ballot_id %}
            {% cycle "red" "blue" %}
        {% else %}
            gray
        {% endifchanged %}
    ">{{ match }}</div>
{% endfor %}
```

-
### include
다른 템플릿을 로드하고 context로 렌더링합니다. (다른 템플릿을 포함시킵니다.)

```
{% include "foo/bar.html" %}
```
문자열 인수는 `./` 또는 `../` 으로 시작하는 상대경로일 수 있습니다.

```
# template_name 변수 사용

{% include template_name %}
```
변수는 context를 받아들이는 render() 메서드가 있는 객체일 수도 있습니다. 이렇게 하면 context에서 컴파일 된 템플릿을 참조 할 수 있습니다.
포함된 템플릿은 템플릿을 포함하는 템플릿 context내에서 렌더링 됩니다.

example) "Hello, John!" 이 출력됩니다.
- context : person이 "John"으로 설정되면 변수greeting은 "Hello"으로 설정됩니다.
- Template : `{% include "name_snippet.html" %}`
- name_snippet.html : `{{ greeting }}, {{ person|default:"friend" }}!`

키워드 인수를 사용하여 템플릿에 추가 context를 전달할 수 있습니다.

```
{% include "name_snippet.html" with person="Jane" greeting="Hello" %}
```
제공된 변수만 사용하려 context를 렌더링하려면 only 옵션을 사용하세요.

```
{% include "name_snippet.html" with greeting="Hi" only %}
```
include한 템플릿이 렌더링되는 동안 예외가 발생하면(누락이나 구문 오류 등) 템플릿 엔진의 디버그 옵션에 따라 동작이 달라집니다.
디버그 모드가 켜져 있으면 `TemplateDoesNotExist` 또는 `TemplateSyntaxError`와 같은 예외가 발생합니다. 디버그 모드가 꺼져 있으면 `{% include %}`는 include 된 템플릿을 렌더링하는 동안 발생하는 예외를 제외하고 빈 문자열을 반환하는 경고를 `django.template` logger에 기록합니다.
>**Note**  
>include 태그는 "하위 템플릿을 렌더링하고 HTML을 포함"의 구현으로 간주되어야하며 "하위 템플릿을 구문 분석하고 해당 하위 템플릿의 내용을 마치 부모의 일부인 것처럼 포함"하지 말아야합니다. 즉, include 된 템플릿간에 **공유 상태가 없음을 의미**합니다. 각 포함은 완전히 독립적인 렌더링 프로세스입니다.  
>block은 include 되기 전에 평가됩니다. 즉, 다른 템플릿의 block을 포함하는 템플릿은 이미 평가되고 렌더링 된 block이 포함됩니다.

### load
custom 템플릿 태그 셋을 로드합니다.

```
# package 안의 somelibrary와 otherlibrary를 로드합니다.

{% load somelibrary package.otherlibrary %}
```

from 인수를 사용하여 라이브러리에서 개별 필터나 태그를 선택적으로 로드할 수 있습니다.

```
# somelibrary에서 foo, bar 템플릿 태그를 로드합니다.

{% load foo bar from somelibrary %}
```

-
### lorem
임의의 "lorem ipsum"라틴 텍스트를 표시합니다.
샘플 데이터를 제공할 때 사용합니다.

```
{% lorem [count] [method] [random] %}
```
{% lorem %} 태그는 3개의 인수를 사용할 수 있습니다.

Argument | Description
---|---
count | 생성할 수(길이)
method | w:단어 / p:HTML단락 / b:일반텍스트단락(기본값)
random | 주어진 임의의 단어는 사용하지 않음

ex)

- `{% lorem %}`는 일반적인 "lorem ipsum"단락을 출력합니다.
- `{% lorem 3 p %}`는 일반적인 "lorem ipsum"단락과 HTML `<p>` 태그로 래핑 된 두 개의 무작위 단락을 출력합니다.
- `{% lorem 2 w random %}`는 두 개의 임의의 라틴어 단어를 출력합니다.

-
### now
주어진 문자열에 따라 혈식을 사용하여 현재의 날짜나 시간을 표시합니다.

```
It is {% now "jS F Y H:i" %}

# 출력 // It is 15th 2월 2017 23:07

It is the {% now "jS \o\f F" %}

# 출력 // It is the 15th of 2월
# 시간을 표시하는 형식이기 때문에 백슬래쉬(\)를 사용하여 문자를 삽입할 수 있습니다.
```
>**Note**  
>전달 된 형식은 `DATE_FORMAT`, `DATETIME_FORMAT`, `SHORT_DATE_FORMAT` 또는 `SHORT_DATETIME_FORMAT` 중 하나일 수 있습니다.

또한 `{% now "Y" as current_year %}`을 사용하여 출력을 변수 안에 저장할 수 있습니다. 이것은 `blocktrans`와 같은 템플릿 태그 안에 `{% now %}`를 사용하고자 할 때 유용합니다

```
{% now "Y" as current_year %}
{% blocktrans %}Copyright {{ current_year }}{% endblocktrans %}
```

-
### regroup
공통 속성에 의해 모든 객체의 목록을 재 그룹화합니다.

예를 들어,

```
cities = [
    {'name': 'Mumbai', 'population': '19,000,000', 'country': 'India'},
    {'name': 'Calcutta', 'population': '15,000,000', 'country': 'India'},
    {'name': 'New York', 'population': '20,000,000', 'country': 'USA'},
    {'name': 'Chicago', 'population': '7,000,000', 'country': 'USA'},
    {'name': 'Tokyo', 'population': '33,000,000', 'country': 'Japan'},
]
```
국가별로 정렬된 계층 구조 목록을 표시하려고 합니다.

- India
 - Mumbai: 19,000,000
 - Calcutta: 15,000,000
- USA
 - New York: 20,000,000
 - Chicago: 7,000,000
- Japan
 - Tokyo: 33,000,000

`{% regroup %}` 태그를 사용하여 국가 별 도시 목록을 그룹화 할 수 있습니다.

```
{% regroup cities by country as country_list %}

<ul>
{% for country in country_list %}
    <li>{{ country.grouper }}
    <ul>
        {% for city in country.list %}
          <li>{{ city.name }}: {{ city.population }}</li>
        {% endfor %}
    </ul>
    </li>
{% endfor %}
</ul>
```
`{% regroup %}`에는 재구성하려는 목록, 그룹화 할 속성 및 결과 목록의 이름이라는 세 가지 인수가 사용됩니다. 여기에서는 country 속성으로 도시 목록을 다시 그룹화하고 `country_list` 결과를 호출합니다.
`{% regroup %}`은 그룹 객체의 목록 (이 경우 country_list)을 생성합니다. 각 그룹 객체에는 두 가지 속성이 있습니다.

- grouper : 그룹화 된 항목 (예 : 문자열 "India"또는 "Japan")
- list : 이 그룹의 모든 항목 목록 (예 : country = 'India'인 모든 도시 목록)

`{% regroup %}`은 입력을 정렬하지 않습니다!
위의 코드는 아래와 같이 출력할 것입니다.

- India
 - Mumbai: 19,000,000
- USA
 - New York: 20,000,000
- India
 - Calcutta: 15,000,000
 USA
 - Chicago: 7,000,000
- Japan
 - Tokyo: 33,000,000

 우리가 원하는 출력이 아닙니다. 해결 방법은 테이터를 표시하려는 방식에 따라 데이터가 정렬되도록 view코드를 확인해야합니다.
 또 다른 방법은 데이터가 dict 목록에 있는 경우 dictsort 필터를 사용하여 정렬할 수 있습니다.
 
```
 {% regroup cities|dictsort:"country" by country as country_list %}
```

-
#### Grouping on other properties(다른 속성에 그룹화하기)
regroup 태그를 이용하여 템플릿의 메소드, 속성, dict key, list item 을 조회할 수 있습니다.

예를 들어 'country'필드가 'description'속성이 있는 클래스의 외래키인 경우 다음을 사용 할 수 있습니다.

```
{% regroup cities by country.description as country_list %}
```
또는 country가 선택 항목이 있는 필드인 경우 `get_FOO_display()`메서드를 속성으로 사용할 수 있기 때문에 선택항목키가 아닌 문자열을 그룹화 할 수 있습니다.

```
{% regroup cities by get_country_display as country_list %}
```
`{{country.grouper}}`는 이제 키가 아닌 선택 항목의 값 필드를 표시합니다.

-

### spaceless

-

### templatetag

-

### url

-

### verbatim

-

### widthratio

-

### with

-

## Built-in filter reference
### add

-

### addslashes

-

### capfirst

-

### center

-

### cut

-

### date

-

### default

-

### default_if_none

-

### dictsort

-

### dictsortreversed

-

### divisibleby

-

### escape

-

### escapejs

-

### filesizeformat

-

### first

-

### floatformat

-

### force_escape

-

### get_digit

-

### iriencode

-

### join

-

### last

-

### length

-

### length_is

-

### linebreaks

-

### linebreaksbr

-

### linenumbers

-

### ljust

-

### lower

-

### make_list

-

### phone2numeric

-

### pluralize

-

### pprint

-

### random

-

### rjust

-

### safe

-

### safeseq

-

### slice

-

### slugify

-

### stringformat

-

### striptags

-

### time

-

### timesince

-

### timeuntil

-

### title

-

### truncatechars

-

### truncatechars_html

-

### truncatewords

-

### truncatewords_html

-

### unordered_list

-

### upper

-

### urlencode

-

### urlize

-

### urlizetrunc

-

### wordcount

-

### wordwrap

-

### yesno

-

## Internationalization tags and filters
### i18n

-

### l10n

-

### tz

-

## Other tags and filters libraries
### django.contrib.humanize

-

### static
#### static

-

#### get_static_prefix

-

#### get_media_prefix