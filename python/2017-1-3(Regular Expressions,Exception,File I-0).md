# 2017-1-3(Regular Expressions,Exception,File I/0)

## 정규표현식 (Regular Expressions)
특정한 패턴에 일치하는 복잡한 문자열을 처리할 때 사용하는 기법.
파이썬에서는 표준 모듈 `re`를 사용해서 정규표현식을 사용할 수 있다.

```python
import re
result = re.match('Lux', 'Lux, the Lady of Luminocity')
```
match의 첫 번째 인자에는 패턴이 들어가며, 두 번째 인자에는 문자열 소스가 들어간다. `match()`는 소스와 패턴의 일치 여부를 확인하고, 일치할 경우 `Match object`를 반환한다.

조금 복잡하거나 자주 사용되는 패턴은 미리 컴파일하여 속도를 향상시킬 수 있다.

```python
pattern1 = re.compile('Lux')
```
컴파일된 패턴객체를 문자열 대신 첫 번째 인자로 사용 가능하다.

### match : 시작부터 일치하는 패턴 찾기

```python
>>> import re
>>> source = 'Lux, the Lady of Luminosity'
>>> m = re.match('Lux', source)
>>> if m:
...   print(m.group())
...
Lux
```
`match()`는 시작부분부터 일치하는 패턴만 찾기 때문에, Lady라는 패턴으로는 찾을 수 없다.

```python
>>> m = re.match('.*Lady', source)
>>> if m:
...   print(m.group())
...
Lux, the Lady
```
위 결과는 다음을 의미한다.

- `.`은 문자 1개를 의미
- `*`는 해당 패턴이 0회 이상 올 수 있다는 의미이다
- 따라서 `.*Lady`는 앞에 아무 문자열(또는 빈) 이후 Lady로 끝나는 패턴을 의미한다

### search : 첫 번째 일치하는 패턴 찾기
`*`패턴 없이 `Lady`만 찾을 경우, 문자열 전체에서 일치하는 부분을 찾는 `search()`를 사용한다.

```python
>>> m = re.search('Lady', source)
>>> if m:
...   print(m.group())
...
Lady
```

### findall : 일치하는 모든 패턴 찾기

```python
>>> m = re.findall('y', source)
>>> m
>>> ['y', 'y']
>>> m = re.findall('y..', source)
>>> m
['y o']
```
끝자리의 `y`는 뒤에 문자가 더 없으므로 포함되지 않으므로, `?`를 추가한다.
`.`은 문자 1개를 의미하며, `?`는 0 또는 1회 반복을 사용한다. `.?`은 문자가 0 또는 1회 올 수 있음을 의미한다.

```pyhton
>>> m = re.findall('y.?.?', source)
>>> m
['y o', 'y']
```

### split : 패턴으로 나누기
문자열의 `split()`메서드와 비슷하지만 패턴을 사용할 수 있다.

```python
>>> m = re.split('o', source)
>>> m
['Lux, the Lady ', 'f Lumin', 'sity']
```

### sub : 패턴 대체하기
문자열의 `replace()`메서드와 비슷하지만 패턴을 사용할 수 있다.

```python
>>> m = re.sub('o', '!', source)
>>> m
'Lux, the Lady !f Lumin!sity'
```

### 정규표현식의 패턴 문자
패턴|문자
---|---
\\d|숫자
\\D|비숫자
\\w|문자
\\W|비문자
\\s|공백 문자
\\S|비공백 문자
\\b|단어 경계 (\w와 \W의 경계)
\\B|비단어 경계

### 정규표현식의 패턴 지정자 (Pattern specifier)
expr은 정규표현식을 말한다

패턴|의미
---|---
abc|리터럴 `abc`
(expr)|expr
expr1 \| expr2 | expr1 또는 expr2
`.` | `\n`을 제외한 모든 문자
`^` | 소스문자열의 시작
`$` | 소스문자열의 끝
expr`?` | 0 또는 1회의 expr
expr`*` | 0회 이상의 최대 expr
expr`*?`| 0회 이상의 최소 expr
expr`+` | 1회 이상의 최대 expr
expr`+?`| 1회 이상의 최소 expr
expr`{m}`| m회의 expr
expr`{m,n}`| m에서 n회의 최대 expr
expr`{m,n}?` | m에서 n회의 최소 expr
[abc] | a or b or c
[^abc] | not (a or b or c)
expr1(?=expr2) | 뒤에 expr2가 오면 expr1에 해당하는 부분
expr1(?=expr2) | 뒤에 expr2까 오지 않으면 expr1에 해당하는 부분
(?<=expr1)expr2 | 앞에 expr1이 오면 expr2에 해당하는 부분
(?<!expr1)expr2 | 앞에 expr1이 오지 않으면 expr2에 해당하는 부분

`\`로 시작하는 패턴 문자나, 정규표현식에서 `\`를 직접 사용해야 하는 경우 문자열의 이스케이프문을 사용하지 않고, 정규식 내에서 `\`로 해석됨을 나타내기 위해 앞에 `r`을 붙인다 (raw string으로 취급된다)
정규표현식의 패턴에는 항상 앞에 `r`을 붙인다고 생각하는 것이 좋다. (만약 정규표현식 내부에서 `\`를 쓰지 않을 경우, `r`을 붙임과 붙이지 않음은 같은 결과를 가져온다)

### 매칭 결과 그룹화

정규표현식 패턴중 괄호로 둘러싸인 부분이 있을 경우, 결과는 해당 괄호만의 그룹으로 저장된다.

Match객체의 `group()`함수는 매치된 **전체 문자열**을 리턴하며, `groups()`함수는 **지정된 그룹 리스트**를 리턴해준다.
`group(0)`은 `group()`과 같은 동작을 하며, `group(숫자)`는 매치된 숫자번째의 그룹요소를 리턴해준다.

```python
s = re.search(r'\w+\w(was)', story)
s.groups()
s.group(0)
s.group(1)
```
`(?P<name>expr)` 패턴을 사용하면 **매칭된 표현식 그룹에 이름을 붙여 사용**할 수 있다.

```python
m = re.search(r'(?P<before>\w+)\s+(?P<was>was)\s+(?P<after>\w+)', story)
m.groups()
m.group('before')
m.group('was')
m.group('after')
```
### 최소일치와 최대일치

```html
<html><body><h1>HTML</h1></body></html>
```
위 항목을

```python
m = re.match(r'<.*>', html)
```
로 검색하면, `.*`표현식이 첫 번째 `>`에서 멈추는것이 아니라 맨 마지막 `>`까지 검색을 진행한다.
`*`이나 `+`에 최소일치인 `?`를 붙여주면, 표현식 다음부분에 해당하는 문자열이 처음 나왔을 때 그 부분까지만 일치시키고 검색을 마친다.

참고사이트  
<http://regexr.com/>  
<http://blog.outsider.ne.kr/360>  

## 예외처리(Exception)
오류가 발생하면 프로그램은 에러를 출력하며 강제종료되거나, 원하지 않는 동작을 한다.

이러한 오류를 안전하게 처리하고, 바로 강제종료 되지 않고 오류 발생 후 처리할 루틴을 실행하고자 할 때 예외처리를 사용한다.

#### 가장 기본적인 형태

```python
try:
    시도할 코드
except:
    에러가 발생했을 경우 실행할 코드
```
#### 여러가지 예외를 구분할 경우

```python
try:
    시도할 코드
except <예외 클래스1>:
    에러클래스 1에 해당할 때 실행할 코드
except <예외 클래스2>:
    ...
except <예외 클래스3>:
    ...
```
#### 예외사항을 변수로 사용할 경우

```python
try:
    시도할 코드
except <예외클래스> as <변수명>:
    <변수명>을 사용한 코드
```
### try ~ else
else문은 try이후 예외가 발생하지 않을 경우 실행된다.

```python
try:
    시도할 코드
except:
    예외 발생시 실행 코드
else:
    예외가 발생하지 않았을 시 실행할 코드
```
### try ~ finally
`finally`문은 `try`이후 예외가 발생하건, 하지않건 무조건 마지막에 실행된다.
### 예외 발생시키기
예외를 발생시킬때는 `raise`구문을 사용한다.
### 예외 만들기
내장 클래스 `Exception`을 상속받아 커스텀 예외를 만들 수 있다. 초기화 메서드에서 예외에서 처리할 데이터를 받고, `print`문으로 사용되고 싶다면 `__str__메서드`를 오버라이드 해준다.

## 파일 입출력
프로그램이 실행되는 동안 데이터는 휘발성 기억장치인 메모리(RAM)에 저장된다. 작업중인 데이터를 저장하거나, 이미 저장되어있는 데이터를 불러오기 위해서는 하드디스크나 SSD에 파일을 쓰거나 읽는 과정이 필요하다.
### 파일 열기

```python
변수 = open(파일명, 모드)
```
내장함수 `open()`을 사용하며, 파일명은 파일의 경로를 나타낸다.

**모드의 첫 번째 글자**

모드|설명
---|---
r|읽기
w|쓰기 (파일이 이미 존재할 경우 덮어쓴다)
x|쓰기 (단, 파일이 존재하지 않을 경우에만)
a|추가 (파일이 존재할 경우 파일의 끝부터 쓴다)

**모드의 두 번째 글자**

모드|설명
---|---
t 또는 없음|텍스트타입
b|이진데이터 타입

**이진데이터**
이진형식(0과 1)로 이루어진 텍스트를 제외한 데이터를 말함.
텍스트 역시 이진데이터의 일종이지만, 데이터의 구조가 인코딩과 개행문자 등 텍스트 형태로 바로 사용할 수 있게 되어있다는 차이가 있다.

**파일을 열고 사용한 뒤에는 파일을 닫아야한다.**
파일을 닫지 않으면 해당파일은 계속 실행이 되어 리소스를 차지하게 된다.

### 파일 쓰기: write()

```python
>>> skills = '''Illumination
... Light Binding
... Prismatic Barrier
... Lucent Singularity
... Final Spark'''
>>>
>>> len(skills)
75
>>> f = open('skills.txt', 'wt')
>>> f.write(skills)
75
>>> f.close()
```
`skills.txt`파일에 내용을 쓴다.

만약 문자열이 클 경우, 일정 단위로 나누어서 파일에 쓰는 방식을 사용한다.

```python
>>> f = open('skills.txt', 'wt')
>>> size = len(skills)
>>> offset = 0
>>> chunk = 30
>>> while True:
...   if offset > size:
...     break
...   f.write(skills[offset:offset+chunk])
...   offset += chunk
...
30
30
15
```
덮어쓰기를 방지하려면 `wt`대신 `xt`를 사용해서 이미 존재하는 파일은 쓸 수 없도록 처리한다.

```python
>>> try:
...   f = open('skills.txt', 'xt')
...   f.write('Other contents')
... except FileExistsError:
...   print('skills.txt exists')
...
skills.txt exists
```
### 텍스트파일 전체 읽기: read()
`read()`함수는 전체 파일을 한 번에 가져오므로, 메모리 사용에 유의해야한다.

```python
>>> f = open('skills.txt', 'rt')
>>> skills = f.read()
>>> f.close()
>>> len(skills)
75
```
한 번에 읽을 크기를 제한하고 싶다면, 인자로 최대 문자수를 입력해준다.

```python
>>> f = open('skills.txt', 'rt')
>>> chunk = 30
>>> while True:
...   part = f.read(chunk)
...   if not part:
...     break
...   skills += part
...
>>> f.close()
>>> len(skills)
75
```
파일을 전부 읽으면 빈 문자열이 리턴되고, `if`문에서 `False`로 판단하여 루프가 끝난다.

### 텍스트파일 줄 단위 읽기: readline()

```python
>>> skills = ''
>>> f = open('skills.txt', 'rt')
>>> while True:
...   line = f.readline()
...   if not line:
...     break
...   skills += line
...
>>> f.close()
>>> len(skills)
75
```
파일을 라인단위로 읽어 문자열에 저장한다.

빈 라인`(\n)`은 길이가 1이며, 파일의 끝에서만 완전히 빈 문자열 `('')`을 리턴한다.

### 이터레이터를 사용한 텍스트 파일 읽기

```python
>>> skills = ''
>>> f = open('skills.txt', 'rt')
>>> for line in f:
...   skills += line
...
>>> f.close()
>>> len(skills)
75
```
`readline()`을 호출한 것과 같은 결과를 보인다.

### 텍스트파일을 줄 단위 문자열 리스트로 리턴: readlines()

```python
>>> f = open('skills.txt', 'rt')
>>> lines = f.readlines()
>>> f.close()
>>> for line in lines:
...   print(line)
...
Illumination

Light Binding

Prismatic Barrier

Lucent Singularity

Final Spark
>>> for line in lines:
...   print(line, end='')
...
Illumination
Light Binding
Prismatic Barrier
Lucent Singularity
Final Spark>>>
```
각 줄에 줄바꿈`(\n)`문자가 있으므로 `print()`함수에 `end`인자를 주어 줄바꿈을 없앨 수 있다.

마지막 라인에는 줄바꿈이 없으므로 인터프리터 프롬프트가 같은 줄에 표시된다.

### 자동으로 파일 닫기: with
연 파일을 닫지 않을 경우, 파이썬에서는 해당 파일이 더 이상 사용되지 않을 때 파일을 자동으로 닫아준다.

다만 메인프로그램이나 오랫동안 동작하는 함수에서 파일을 열 경우, 명시적으로 닫아주지 않을 경우 문제가 발생한다.

```python
with 표현식 as 변수
```
위의 구문을 사용하면, with문 내부에서 파일을 사용한 후 구문이 종료되면 자동으로 파일을 닫아주므로 프로그래밍 단계에서 일일이 파일을 닫아주는 부분에 신경쓰지 않아도 된다.

```python
>>> with open('skills.txt', 'wt') as f:
...   f.write(skills)
```

### 이진데이터 다루기
쓰거나 읽을 때, `t`대신 `b`인자를 사용하면 된다.