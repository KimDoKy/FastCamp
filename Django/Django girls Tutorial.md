# Django

## 가상환경 설정
`pip list`

`pyenv virtualenv 3.4.3 폴더명`

`pyenv local 폴더명(프로젝트명)`

`pyenv versions`

`vi ./gitignore` : Git에서 필요 없는 파일 무시하기 셋팅 [https://www.gitignore.io/](https://www.gitignore.io/api/git%2Cpycharm%2Cpython python,django,git) 에서 소스를 생성하여 넣는다.

`pip freeze > requirements.txt`

`python -m django --version` : 장고 버젼 확인

`$ django-admin startproject mysite` : 현재 디렉토리에서 mysite 라는 디렉토리를 생성

**django-admin.py**은 **스크립트로 디렉토리와 파일들을 생성**

```
djangogirls
├───manage.py
└───mysite
        settings.py
        urls.py
        wsgi.py
        __init__.py
```

**manage.py** 파일 또한 스크립트인데, 사이트 관리를 도와주는 역할을 합니다.  
이 스크립트로 다른 설치 작업 없이, 컴퓨터에서 웹 서버를 시작할 수 있습니다.

**settings.py**는 웹사이트 설정이 있는 파일

**urls.py** 파일은 urlresolver가 사용하는 패턴 목록을 포함 

### 설정 변경
`mysite/settings.py`

시간대 변경  
```
TIME_ZONE = 'Asia/Seoul'
```  
>[위키피디아 타임존 리스트](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) 참조

```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```  
>정적파일 경로를 추가(CSS 관련 셋팅)

### 데이터베이스 설정하기
`mysite/settings.py`

```
DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }
```

**`python manage.py migrate`**
>데이터베이스 생성

```
(myvenv) ~/djangogirls$ python manage.py migrate
Operations to perform:
  Synchronize unmigrated apps: messages, staticfiles
  Apply all migrations: contenttypes, sessions, admin, auth
Synchronizing apps without migrations:
   Creating tables...
      Running deferred SQL...
   Installing custom SQL...
Running migrations:
  Rendering model states... DONE
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying sessions.0001_initial... OK
```
```
$ python manage.py runserver
```
>웹서버 실행

http://127.0.0.1:8000/ 접속
![](https://tutorial.djangogirls.org/ko/django_start_project/images/it_worked2.png)

`control + c` : 서버 중지

## Django 모델

### 어플리케이션 제작하기
`python manage.py startapp blog`
>프로젝트 내부에 별도의 어플리케이션 생성

```
djangogirls
├── mysite
|       __init__.py
|       settings.py
|       urls.py
|       wsgi.py
├── manage.py
└── blog
    ├── migrations
    |       __init__.py
    ├── __init__.py
    ├── admin.py
    ├── models.py
    ├── tests.py
    └── views.py
```


`mysite/settings.py`

```
    INSTALLED_APPS = (
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'blog',	# blog 추가
    )
```
>어플리케이션을 생성한 후 장고에게 사용해야한다고 알림

### 블로그 글 모델 만들기
모든 Model 객체는 blog/models.py 파일에 선언하여 모델을 만듭니다. 

```
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey('auth.User')
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
        default=timezone.now)
    published_date = models.DateTimeField(
        blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

>언더스코어(_). 파이썬에서 자주 사용되는 관습.
>"던더(dunder; 더블-언더스코어의 준말)"

#### 코드 해석
from 또는 import로 시작하는 부분은 다른 파일에 있는 것을 추가하라는 뜻입니다.
class Post(models.Model):는 모델을 정의하는 코드입니다. (모델은객체(object))

class는 특별한 키워드로, 객체를 정의한다는 것을 알려줍니다.
Post는 모델의 이름입니다. 항상 클래스 이름의 첫 글자는 대문자로 써야 합니다.
models.Model은 Post가 장고 모델임을 의미합니다. 이 코드 때문에 장고는 Post가 데이터베이스에 저장되어야 된다고 알게 됩니다.
속성을 정의하는 것
title, text, created_date, published_date, author  
속성을 정의하기 위해, 각 필드마다 어떤 종류의 데이터 타입을 가지는지를 정해야해요.  
여기서 데이터 타입에는 텍스트, 숫자, 날짜, 유저 같은 다른 객체 참조 등이 있습니다.

models.CharField - 글자 수가 제한된 텍스트를 정의할 때 사용합니다. 글 제목같이 대부분의 짧은 문자열 정보를 저장할 때 사용합니다.
models.TextField - 글자 수에 제한이 없는 긴 텍스트를 위한 속성입니다. 블로그 콘텐츠를 담기 좋겠죠?
models.DateTimeField - 이것은 날짜와 시간을 의미합니다.
models.ForeignKey - 다른 모델이 대한 링크를 의미합니다.

def publish(self): publish라는 메서드(method) 입니다. def는 이 것이 함수/메서드라는 뜻이고, publish는 메서드의 이름입니다.

메서드는 자주 무언가를 되돌려주죠. (return) 그 예로 __str__ 메서드를 봅시다. 이 시나리오대로라면, __str__를 호출하면 Post 모델의 제목 텍스트(string) 를 얻게 될 거에요.

### 데이터베이스에 모델을 위한 테이블 만들기
`python manage.py makemigrations blog`
> 장고 모델에 변화가 생김을 알림

```
(myvenv) ~/djangogirls$ python manage.py makemigrations blog
Migrations for 'blog':
  0001_initial.py:
  - Create model Post
```
장고는 데이터베이스에 지금 반영할 수 있도록 **마이그레이션 파일(migration file)**이라는 것을 준비해둠

`python manage.py migrate blog`
> 실제 데이터베이스에 모델 추가를 반영

```
(myvenv) ~/djangogirls$ python manage.py migrate blog
Operations to perform:
  Apply all migrations: blog
Running migrations:
  Rendering model states... DONE
  Applying blog.0001_initial... OK
```

## Django 관리자
모델링한 글들을 장고 관리자에서 추가하거나 수정, 삭제

`blog/admin.py`

```
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```
>Post 모델을 import)
>관리자 페이지에서 만든 모델을 보려면 admin.site.register(Post)로 모델을 등록해야함

`python manage.py runserver` // 서버 실행  
`http://127.0.0.1:8000/admin/` // 어드민 동작 확인

![](https://tutorial.djangogirls.org/ko/django_admin/images/login_page2.png)

#### 로그인을 하기 위해서는, 모든 권한을 가지는 **슈퍼유저(superuser)**를 생성

`python manage.py createsuperuser`

```
(myvenv) ~/djangogirls$ python manage.py createsuperuser
Username: admin
Email address: admin@admin.com
Password:
Password (again):
Superuser created successfully.
```

로그인 후
![](https://tutorial.djangogirls.org/ko/django_admin/images/django_admin3.png)

2 ~3 개 블로그 포스트를 올려보세요. 
![](https://tutorial.djangogirls.org/ko/django_admin/images/edit_post3.png)

## Django urls
`mysite/urls.py`

```
url(r'^admin/', include(admin.site.urls)),
```
>admin/으로 시작하는 모든 URL을 장고가 view와 대조해 찾아낸다는 뜻

### 나의 첫 번째 Django url!
`mysite/urls.py`파일을 깨끗한 상태로 유지하기 위해, blog 어플리케이션에서 메인 **mysite/urls.py**파일로 url들을 가져올 거에요.  
main url ('')로 blog.urls를 가져오는 행을 추가해 봅시다.

```
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'', include('blog.urls')),
]
```
지금 장고는 'http://127.0.0.1:8000/'로 들어오는 모든 접속 요청을 blog.urls로 전송하고 추가 명령을 찾을 거예요.

### blog.urls
`blog/urls.py`이라는 새 파일을 생성

```
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.post_list, name='post_list'),
]
```
>장고의 메소드와 blog 어플리케이션에서 사용할 모든 views들을 불러오고 있어요.

`ost_list라는 이름의 view가 ^$ URL`에 할당되었습니다.  
이 정규표현식은 `^`에서 시작해 `$`로 끝나는 지를 매칭할 것입니다. 즉 문자열이 아무것도 없는 경우만 매칭하겠죠.
마지막 부분인 name='post_list' 는 URL에 이름을 붙인 것으로 뷰를 식별합니다.

'http://127.0.0.1:8000/' 으로 접속해 결과를 확인
![](https://tutorial.djangogirls.org/ko/django_urls/images/error1.png)

페이지에서 no attribute 'post_list'라는 오류가 보일 거에요. post_list에서 혹시 떠오르는 것이 있나요? 바로 뷰(view) 를 말하는 거죠! 모두 준비가 되었다는 뜻이에요. 아직 view를 안 만들었다는 것만 빼고요.

## Django 뷰 만들기
**뷰(view)** 는 **어플리케이션의 "로직"을 넣는 곳**  
모델에게서 필요한 정보를 받아와서 템플릿에 전달하는 역할  
뷰는 views.py 파일 안에 정의됨.

`blog/views.py`

```
from django.shortcuts import render


def post_list(request):
    return render(request, 'blog/post_list.html', {})
```
>`post_list`라는 메서드를 만들었습니다.   
이 메서드는 요청(request)을 넘겨받아 render 메서드를 호출합니다.  
render 메서드는 넘겨진 요청(request)과 blog/post_list.html 템플릿 받아 리턴된 내용이 브라우저에 보여지게 됩니다.

 http://127.0.0.1:8000/로 접속해 결과를 확인
 ![](https://tutorial.djangogirls.org/ko/django_views/images/error.png)
 
## 첫번째 템플릿!
템플릿은 `blog/templates/blog` 디렉토리에 저장

디렉토리 생성

```
blog
└───templates
    └───blog
```
`blog/templates/blog` 디렉토리에 `post_list.html` 파일을 생성

웹사이트 확인 : http://127.0.0.1:8000/
![](https://tutorial.djangogirls.org/ko/html/images/step1.png)
>TemplateDoesNotExists 에러가 나온다면, 웹 서버를 다시 시작하세요. 커맨드라인(혹은 콘솔창)으로 가서 Ctrl + C를 눌러 웹 서버 작동을 멈춥니다. 그런 후 다시 python manage.py runserver 명령을 실행해 서버를 재시작합니다.

`post_list.html`

```
<html>
<head>
    <title>Django Girls blog</title>
</head>
<body>
<div>
    <h1><a href="">Django Girls Blog</a></h1>
</div>

<div>
    <p>published: 14.06.2014, 12:14</p>
    <h2><a href="">My first post</a></h2>
    <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.</p>
</div>

<div>
    <p>published: 14.06.2014, 12:14</p>
    <h2><a href="">My second post</a></h2>
    <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut f.</p>
</div>
</body>
</html>
```
확인 : http://127.0.0.1:8000/
![](https://tutorial.djangogirls.org/ko/html/images/step6.png)


## Django ORM과 QuerySets
장고를 데이터베이스에 연결, 데이터를 저장하는 방법

### 쿼리셋(QuerySet)
쿼리셋은 전달받은 모델의 객체 목록입니다. 쿼리셋은 데이터베이스로부터 데이터를 읽고, 필터를 걸거나 정렬을 할 수 있습니다.

### 장고 쉘(shell)
```
python manage.py shell
```

```
>>> from blog.models import Post	#Post모델을 blog.models에서 불러왔어요.
>>> Post.objects.all()
[<Post: my post title>, <Post: another post title>]	#장고 관리자 인터페이스로 만들었던 글 리스트
>>> from django.contrib.auth.models import User	#User 모델을 불러옵니다.
>>> User.objects.all()
[<User: admin>]	#User확인
me = User.objects.get(username='admin')	#사용자의 인스턴스를 가져옴
>>> Post.objects.create(author=me, title='Sample title', text='Test')	#새글 객체를 저장(게시물 만들기)
>>> Post.objects.all()
[<Post: my post title>, <Post: another post title>, <Post: Sample title>]	#정상동작확인. 먼저 입력했던 모든 글 출력
>>> Post.objects.filter(author=me)
[<Post: Sample title>, <Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>]	#필터링(작성자검색)
>>> Post.objects.filter( title__contains='title' )
[<Post: Sample title>, <Post: 4th title of post>]	#제목으로 검색
#출판 날짜(published_date)가 과거인 글들을 필터링
>>> from django.utils import timezone
>>> post = Post.objects.get(title="Sample title")	#게시하려는 게시물의 인스턴스 얻기
>>> post.publish()	#publish 메서드를 사용해서 출판
>>> Post.objects.filter(published_date__lte=timezone.now())
[<Post: Sample title>]	#게시된 글의 목록 출력
```
>`title`와 `contains` 사이에 있는 밑줄(`_`)이 2개입니다. 장고 ORM은 필드 이름(`"title"`)과 연산자과 필터(`"contains"`)를 밑줄 2개를 사용해 구분합니다. 밑줄 1개만 입력한다면, `"FieldError: Cannot resolve keyword title_contains"`라는 오류가 뜰 거에요.

### 정렬하기
**퀘리셋**은 **객체 목록을 정렬**도 할 수 있어요. 이제 `created_date` 필드를 정렬

```
>>> Post.objects.order_by('created_date')
[<Post: Sample title>, <Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>]
```
`-`을 맨 앞에 붙여주면 **내림차순**으로 정렬도 가능

```
>>> Post.objects.order_by('-created_date')
[<Post: 4th title of post>,  <Post: My 3rd post!>, <Post: Post number 2>, <Post: Sample title>]
```

### 쿼리셋(QuerySets) 연결하기
쿼리셋들을 함께 연결(chaining)

```
>>> Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
```
쉘을 종료

```
>>> exit()
```

## 템플릿의 동적 데이터
블로그 글이 각각 다른 장소에 조각조각 나눠져있어요. `Post` 모델은 `models.py`파일에 정의되어 있고, `post_list` 모델은 `views.py` 파일에 정의되어 있습니다. 템플릿도 추가해야해요. 하지만 실제로 HTML 템플릿에서 글들이 어떻게 보여질까요? 보여주고자 하는 컨텐츠(데이터베이스 안에 저장되어 있는 모델들) 를 가져와서 우리의 템플릿에 넣어서 멋있게 보여줄 것입니다.
`views` 가 모델과 템플릿을 연결하는 역할을 한다는 것을 알 수 있어요. `post_list view` 에서 보여주고 템플릿에 넘기기 위해서 모델을 가져와야해요. 그래서 기본적으로 `view` 에서는 템플릿에서 무엇을 보여줄 지 (모델) 을 선택을 하지요.

`blog/views.py`

```
#...
from .models import Post 
```
> models.py 파일에서 정의한 모델을 가져올 때

### 쿼리셋(QuerySet)
Post 모델에서 실제 블로그 글들을 가져와야 하는데 이때 필요한 특별한 것이 바로 **QuerySet**입니다.

`blog/views.py` 파일의 `def post_list(request)` 함수에다 코드 조각을 넣어봅시다.

```
from django.shortcuts import render
    from django.utils import timezone
    from .models import Post

    def post_list(request):
        posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
        return render(request, 'blog/post_list.html', {'posts': posts})
```
출판된 날짜(published_date) 기준으로 정렬 코드 추가
`ender`함수에서는 이미 요청 시 매개변수`request`와 템플릿 파일 `'blog/post_list.html'`를 가지고 있습니다. `{}` 와 같이 보이는 마지막 매개 변수는 템플릿을 사용하기 위해 무엇인가를 추가하는 장소입니다. 이름을 넣어줘야 합니다. (여기서는 'posts'이에요.) 이렇게 보일지도 모르겠어요. `: {'posts': posts}. :`
			

## Django 템플릿
데이터를 보여줄 차례에요! 이를 위해 장고는 내장된 `template tags` 라는 유용한 기능을 제공

### 장고 템플릿 태그(Django template tags) 
파이썬을 HTML로 바꿔주어, 빠르고 쉽게 동적인 웹사이트를 만들 수 있게 도와주어요.

### post 목록 템플릿 보여주기
넘겨진 posts 변수를 받아서 HTML에 나타나도록 
장고 템플릿 안에 있는 값을 출력하려면, 변수 이름안에 중괄호를 넣어 표시해야합니다.

```
{{ posts }}
```

`blog/templates/blog/post_list.html` 템플릿에서 하세요. 

```
<html>
<head>
    <title>Django Girls blog</title>
</head>
<body>
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
</body>
</html>
```
![](https://tutorial.djangogirls.org/ko/django_templates/images/step3.png)



-----
CSS 추가 예정.
글 다듬기 예정


정규표현식(Regex)

장고가 URL을 뷰에 매칭시키는 방법이 궁금하죠? 이 부분은 조금 까다로울 수 있어요. 장고는 regex를 사용하는데, "정규표현식(regular expressions)"의 줄임말입니다. 정규식은 정말 (아주!) 많은 검색 패턴의 규칙을 가지고 있어요. 정규식은 심화 내용이기 때문에, 자세한 내용은 다루지 않을 거예요.

패턴을 만드는 방법이 궁금하다면, 아래에 있는 표기법을 확인하세요. 우리는 패턴을 찾는데 필요한 몇 가지 규칙만 필요합니다. :

^ 문자열이 시작할 때
$ 문자열이 끝날 때
\d 숫자
+ 바로 앞에 나오는 항목이 계속 나올 때
() 패턴의 부분을 저장할 때
이외에 url 정의는 문자적으로 만들 수 있어요.

이런 사이트 주소가 있다고 해봅시다. : http://www.mysite.com/post/12345/ 여기에서 12345는 글 번호를 의미합니다.

뷰마다 모든 글 번호을 작성하는 것은 정말 힘든 일이 될 거에요. 정규 표현식으로 url과 매칭되는 글 번호를 뽑을 수 있는 패턴을 만들 수 있어요. 이렇게 말이죠. : ^post/(\d+)/$. 어떤 뜻인지 하나씩 나누어 어떤 뜻인지 알아볼게요. :

^post/는 장고에게 url 시작점에 (오른쪽부터) post/가 있다는 것을 말해 줍니다. ^)
(\d+)는 숫자(한 개 또는 여러개) 가 있다는 뜻입니다. 내가 뽑아내고자 글 번호가 되겠지요.
/는 장고에게 /뒤에 문자가 있음을 말해 줍니다.
$는 URL의 끝이 방금 전에 있던 /로 끝나야 매칭될 수 있다는 것을 나타냅니다.
