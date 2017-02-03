# 2017-2-2(Class,virtualenv Setting)

## virtualenv Setting

`pip freeze` : 현재 설치된 정확한 버젼

`pip freeze > requirements.txt` : 설치된 버전(작동환경)의 패키지들을 패키지리스트 문서로 기록한다.

`pyenv uninstall fc-python` : 가상환경 삭제

`pip list` : 설치된 패키지 확인

`pyenv virtualenv 3.4.3 폴더명` : 지정한 폴더를 가상환경으로 선언

`pyenv local 폴더명(프로젝트명)` : 지정한 폴더에 가상환경 적용선언

`pyenv versions` : 셋팅확인

`git init` : 해당 폴더 Git 저장소로 지정

`vi ./gitignore` : Git에서 필요 없는 파일 무시하기 셋팅
<https://www.gitignore.io/> 에서 소스를 생성하여 넣는다.
>
<https://www.gitignore.io/api/git%2Cpycharm%2Cpython>
python,django,git 설정 소스

`pip install --upgrade pip` : pip upgrade

`pip install requests` : requests 설치

`pip install BeautifulSoup4` : BeautifulSoup4 install (crawling)

`alias py="open -a /Applications/PyCharm\ CE.app/Contents/MacOS/pycharm"` : py . 으로 터미널에서도 바로 실행하도록 셋팅

파이참 가상환경 셋팅

`설정 - project:폴더명 - project Interpreter`

/usr/local/var/pyenv/versions/3.4.3/envs/pip_test_env/bin/python

git push origin master -force

`git reset HEAD^` : Git commit 취소

`rm -rf .git` : Git 삭제

`pip install -r requirements.txt` : requirements으로 패키지 설치
 
`pip install --upgrade pip` : ImportError: No module named 'packaging' 오류 발생시

`pip install -U setuptools` : Could not open requirements file 오류시.

 
 
## 클래스(class)
### 객체지향 프로그래밍
파이썬의 모든것은 객체이며, 객체를 사용할 때는 변수에 해당 객체를 **참조(Reference)**시켜 사용한다. **인스터스(Instance)**는 **속성(attribute)**와 **메서드(method)**를 갖는다.

### 클래스(class)
> 인스턴스(Instance) : class를 통해 생성된 객체

```python
class Shop:	# 클래스 선언
	def __init__(self,name):	# 클래스 초기화
		self.name = name
```
```python
from 모듈명 import 클래스명		# 모듈 import
macdonald = Shop('Macdonald')	# class를 통해 instance 생성
```
#### 동작과정
1.Shop 클래스가 정의되었는지 찾는다.
2.Shop 클래스형 객체를 메모리에 생성한다.
3.생성한 객체의 초기화 메서드 __init__을 호출한다.
4.name값을 저장하고, 만들어진 객체를 반환한다.
5.macdonald변수에 반환된 객체를 할당한다.

self : 객체의 매소드를 정의할 때, 첫번째 인수는 항상 `self`이다. `self`에서는 메서드를 호출하는 객체 자신이 자동으로 전달된다. 클래스의 매서드를 사용할 때 어떤 객체가 해달 매서드를 사용하고 있는지 알 수 있도록 하기 위함과 **하나의 매서드를 여러 객체가 공유**할 수 있게 함이다.

#### 클래스 속성

어떤 하나의 클래스로부터 생성된 객체들이 같은 값을 가지게 하고 싶을 경우, **클래스 속성(class attribute)**를 사용한다.

```python
class Shop:
    description = 'Python Shop Class'
    def __init__(self, name):
        self.name = name
```
객체들에게서 각각의 인스턴스와는 별개의 공통된 메서드를 사용하게 하고 싶을 경우 **클래스 메서드(class method)**를 사용한다.



### 메서드(Method)
#### 인스턴스 메서드
인스턴스 메서드는 첫 번째 인수로 `self`를 가진다. 인스턴스를 이용해 메서드를 호출할 때 호출한 인스턴스가 자동으로 전달되며, 전달받은 인스턴스는 상태를 확인하거나 조작하는데에 사용된다.

#### 클래스 메서드
클래스 메서드는 클래스 속성에 대해 동작하는 메서드이다. 위의 인스턴스 메서드와 달리 **호출 주체가 클래스**이며, 첫 번째 인자도 클래스이다.
만약 인스턴스가 첫 번째 인자로 주어지더라도 해당 인자의 클래스로 자동으로 바뀌어 전달된다.
클래스메서드는 `@classmethod`데코레이터를 붙여 선언하며, 첫 번째 인자의 이름은 관용적으로 `cls`를 사용한다.

#### 스태틱 메서드
스태틱 메서드는 **클래스 내부에 정의된 일반 함수**이며, 단지 클래스나 인스턴스를 통해서 접근할 수 있을 뿐 **해당 클래스나 인스턴스에 영향을 주는 것은 불가능**하다.
스태틱메서드는 `@staticmethod`데코레이터를 붙여 선언한다.

### 속성 접근 지정자 (attribute access modifier)
#### 캡슐화
객체를 구현할 때, 사용자가 반드시 알아야 할 데이터나 메서드를 제외한 부분을 **은닉**시켜 정해진 방법을 통해서만 객체를 조작할 수 있도록 하는 방식.
객체의 데이터나 메서드의 은닉 정도를 결정할 때, **속성 접근 지정자**를 사용한다.
속성 이름을 `__`로 시작하면, 외부에서의 접근을 제한한다. 이 경우를 `private 지정자`라고 한다.
파이썬은 속성을 실제로 사용하지 못하도록 숨기지 않고, **네임 맹글링(name mangling)**이라는 기법을 사용한다. 파이썬에서는 문법적으로 private데이터에 대한 접근을 막는 법을 제공하지는 않는다.
이는 개발자에게 최대한 제약을 가하지 않는다는 파이썬의 철학때문이다.

### get/set속성값과 프로퍼티
파이썬에서는 지원하지 않지만, 어떤 언어들은 외부에서 접근할 수 없는 **private객체 속성**을 지원한다. 이 경우, 객체에서는 해당 속성을 읽고 쓰기 위해 **getter, setter메서드**를 사용해야 한다.

파이썬에서는 해당 기능을 **프로퍼티(property)**를 사용해 간편히 구현한다.

실제 getter, setter를 구현해본다.
```python
@property
def name(self):
    return self.__name

@name.setter
def name(self, new_name):
    self.__name = new_name
    print('Set new name ({})'.format(self.__name))
```
`setter`프로퍼티를 명시하지 않으면 읽기전용이 되어 외부에서 조작할 수 없게 된다.

### 상속 (Inheritance)
거의 비슷한 기능을 수행하나, **약간의 추가적인 기능**이 필요한 다른 클래스가 필요할 경우 기존의 클래스를 상속받은 새 클래스를 사용하는 형태로 문제를 해결할 수 있다.

이 때, 상속 되는 클래스를 **부모(상위)클래스**라고 하며, 상속을 받는 클래스는 **자식(하위)클래스**라고 한다.

상속을 받을때는 클래스의 정의 다음 괄호에 부모 클래스를 적어주면 된다.

```python
class Restaurant(Shop):
    pass
```
상속받은 클래스는 부모 클래스의 모든 속성과 메서드를 사용할 수 있다.

#### 메서드 오버라이드
상속받은 클래스에서, 부모 클래스의 메서드와는 다른 동작을 하도록 할 수 있다. 이 경우 부모 클래스의 메서드를 덮어씌워서 사용하도록 하며, 이 방법을 **메서드 오버라이드(method override)**라고 한다.

#### 부모 클래스의 메서드를 호출 (super)
자식클래스의 메서드에서 부모 클래스에서 사용하는 메서드의 전체를 새로 쓰는것이 아닌, 부모 클래스의 메서드를 호출 후 해당 내용으로 새로운 작업을 해야 할 경우 **super()메서드**를 사용해서 부모 클래스의 메서드를 직접 호출할 수 있다.
```python
class Restaurant(Shop):
    def __init__(self, name, shop_type, address, rating):
        super().__init__(name, shop_type, address)
        self.rating = rating
```
> `super()`메서드를 사용해서 부모의 `__init__ 메서드`를 호출한다.

### 다형성과 덕 타이핑
파이썬은 **다형성(polymorphism)**을 **덕 타이핑(duck typing)**이라는 방식으로 구현한다.
다형성을 지원하는 프로그래밍 언어에서는 **같은 이름의 함수에서 서로 다른 기능을 하는 방식**으로 프로그래밍을 할 수 있다.
파이썬에서는 각 객체의 타입과 전혀 상관없이 해당 객체에서 만든 같은 이름의 메서드들을 실행할 수 있다.

-

## 인터프리터로 클래스 테스트할 때 tip

```
%load_ext autoreload
%autoreload 2
```
인터프리터 환경에서 작업시 클래스 변경후 인스턴스의 매서드가 정상 작동하지 않을때 입력할 값.
입력후 다시 import해야 함.


## 장고 튜토리얼 가상환경 설정하기.
```
pip install django
```