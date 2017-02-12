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


## CSS - 예쁘게 만들기

**부트스트랩(Bootstrap)**은 예쁜 웹사이트를 개발하기 위해 사용되는 가장 유명한 HTML과 CSS 프레임워크입니다: https://getbootstrap.com/

### Bootstrap 설치하기

Bootstrap을 설치하려면, .html 파일의 <head> 안에 이 링크를 넣어야 합니다. (blog/templates/blog/post_list.html):

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
```
### Django 정적 파일(static files)

blog앱에 정적파일을 추가하면 됩니다.
blog 앱 안에 static폴더를 만드세요. :

```
djangogirls
├── blog
│   ├── migrations
│   └── static
└── mysite
```
장고는 app폴더 안에 있는 "static" 폴더를 자동으로 찾아 안에 있는 내용을 불러낼 거에요.

static디렉토리 안에 css라고 새로운 디렉토리를 만드세요. 그리고 css디렉토리 안에 blog.css라는 파일을 만드세요.

```
djangogirls
└─── blog
     └─── static
          └─── css
               └─── blog.css
```
> CSS 학습 [Codeacademy HTML & CSS course](https://www.codecademy.com/learn/web)
> 색깔 참조 <http://www.colorpicker.com/>  
> [w3school](http://www.w3schools.com/colors/colors_names.asp)

post_list.html 추가

```html
{% load staticfiles %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
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
확인

![](https://tutorial.djangogirls.org/ko/css/images/color2.png)

 css 소스 추가
 
 ```css
 .page-header {
    background-color: #ff9400;
    margin-top: 0;
    padding: 20px 20px 20px 40px;
}

.page-header h1, .page-header h1 a, .page-header h1 a:visited, .page-header h1 a:active {
    color: #ffffff;
    font-size: 36pt;
    text-decoration: none;
}

.content {
    margin-left: 40px;
}

h1, h2, h3, h4 {
    font-family: 'Lobster', cursive;
}

.date {
    float: right;
    color: #828282;
}

.save {
    float: right;
}

.post-form textarea, .post-form input {
    width: 100%;
}

.top-menu, .top-menu:hover, .top-menu:visited {
    color: #ffffff;
    float: right;
    font-size: 26pt;
    margin-right: 20px;
}

.post {
    margin-bottom: 70px;
}

.post h1 a, .post h1 a:visited {
    color: #000000;
}
```
post_list.html 최종 수정

```html
<div class="content container">
    <div class="row">
        <div class="col-md-8">
            {% for post in posts %}
                <div class="post">
                    <div class="date">
                        {{ post.published_date }}
                    </div>
                    <h1><a href="">{{ post.title }}</a></h1>
                    <p>{{ post.text|linebreaksbr }}</p>
                </div>
            {% endfor %}
        </div>
    </div>
</div>
```
![](https://tutorial.djangogirls.org/ko/css/images/final.png)

## 템플릿 확장하기(template extending)
웹사이트 안의 서로 다른 페이지에서 HTML의 일부를 동일하게 재사용
이 방법을 사용하면 동일한 정보/레이아웃을 사용하고자 할 때, 모든 파일마다 같은 내용을 반복해서 입력 할 필요가 없게 됩니다. 또 뭔가 수정할 부분이 생겼을 때, 각각 모든 파일을 수정할 필요 없이 딱 한번만 수정하면 된답니다!
### 기본 템플릿 생성하기
기본 템플릿은 웹사이트 내 모든 페이지에 확장되어 사용되는 가장 기본적인 템플릿입니다.
blog/templates/blog/에 base.html 파일을 만들어 봅시다:

```
blog
└───templates
    └───blog
            base.html
            post_list.html
```
post_list.html에 있는 모든 내용을 base.html에 아래 내용을 복사해 붙여넣습니다. 그리고 <body>의 내용을 수정합니다.

```html
<body>
    <div class="page-header">
        <h1><a href="/">Django Girls Blog</a></h1>
    </div>
    <div class="content container">
        <div class="row">
            <div class="col-md-8">
            {% block content %}
            {% endblock %}
            </div>
        </div>
    </div>
</body>
```
`{% for post in posts %}{% endfor %}` 사이에 있는 모든 내용을 바꿨습니다. :

```
{% block content %}
{% endblock %}
```
block은 템플릿 태그로 base.html을 확장한 이 블럭에 HTML을 추가할 수 있게 해줍니다. 

blog/templates/blog/post_list.html 파일을 다시 엽니다. body 태그 안에 있는 모든 내용을 지우고, <div class="page-header"></div>도 지웁니다.

```html
{% for post in posts %}
    <div class="post">
        <div class="date">
            {{ post.published_date }}
        </div>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```
파일 맨 윗 줄에 아래 내용을 추가합니다.

```
{% extends 'blog/base.html' %}
```
post_list.html 파일은 이제 base.html 템플릿을 확장했음을 의미합니다. {% block content %}과 {% endblock content %} 사이에 (우리가 방금 추가한 행들을 빼고) 모든 것을 넣습니다. 

최종본
```
{% extends 'blog/base.html' %}

{% load staticfiles %}
<html>
<head>
    <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
    <link href="https://fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext" rel="stylesheet" type="text/css">
    <title>Django Girls blog</title>
</head>
<body>
{% block content %}
    {% for post in posts %}
        <div class="post">
            <div class="date">
                {{ post.published_date }}
            </div>
            <h1><a href="">{{ post.title }}</a></h1>
            <p>{{ post.text|linebreaksbr }}</p>
        </div>
    {% endfor %}
{% endblock content %}
</body>
</html>
```
>만약 `TemplateDoesNotExists` 에러가 난다면 `blog/base.html` 파일이 없어 콘솔에서 `runserver`가 작동되고 있는 경우 입니다. 이런 경우, (Ctrl+C)를 눌러서 멈춘 후 python manage.py runserver명령어로 **재시작**해 주세요.

## 프로그램 어플리케이션 확장하기
### Post에 템플릿 링크 만들기
blog/templates/blog/post_list.html 파일에 링크를 추가하는 것부터 시작

post 목록에 있는 제목에서 post의 내용 페이지로 가는 링크를 만들 거에요. <h1><a href="">{{ post.title }}</a></h1> 를 변경해 봅시다. post의 상세 페이지는 로 연결됩니다.

```
<h1><a href="{% url 'post_detail' pk=post.pk %}">{{ post.title }}</a></h1>
```
{% url 'post_detail' pk=post.pk %}에 대해 설명
{% %} 표기는 장고 템플릿 태그를 사용하고 있는 것
blog.views.post_detail는 우리가 만들려고 하는 경로인 post_detail view 입니다. 

http://127.0.0.1:8000/를 열어보세요. 오류 메세지가 나올 거에요. (예상대로, 아직 post_detail을 위한 view 파일 만들지 않아 오류가 나는 것이죠.)
![](https://tutorial.djangogirls.org/ko/extend_your_application/images/no_reverse_match2.png)

### Post 상세 페이지에 URL 만들기
`urls.py` 파일에 `post_detail view` 를 위한 URL를 만들어 봅시다!

첫 번째 게시물의 상세 URL 은 이렇게 나올 거에요.  
`http://127.0.0.1:8000/post/1/`

`blog/urls.py`파일에 URL을 만들어, 장고가 `post_detail`이란 `view` 로 보내, 전체 블로그 글이 보일 수 있게 만들어 봅시다. `url(r'^post/(?P<pk>[0-9]+)/$', views.post_detail, name='post_detail')` 코드를 `blog/urls.py` 파일에 추가하세요.

```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.post_list, name='post_list'),
    url(r'^post/(?P<pk>[0-9]+)/$', views.post_detail, name='post_detail'),
]
```
```
^post/(?P<pk>[0-9]+)/$
```

- `^`은 "시작"을 뜻합니다.
- 첫 부분 다음부터 나오는 post/는 URL이 post 를 포함해야한다는 것을 뜻합니다.
- `(?P<pk>[0-9]+)` - 이 부분은 좀 까다롭습니다. 이 정규 표현식의 의미는 장고가 여러분이 여기에 넣은 모든 것을 pk변수에 넣어 뷰로 전송하겠다는 뜻입니다. [0-9]은 문자를 제외하고, 숫자 0부터 9까지 하나의 숫자만 있다는 뜻입니다. +는 하나 또는 그 이상의 숫자가 와야한다는 것을 의미합니다. 따라서 http://127.0.0.1:8000/post//라고 하면 post/ 다음에 숫자가 없기 때문에 해당이 안되지만, http://127.0.0.1:8000/post/1234567890/는 완벽하게 매칭됩니다.
- `/` - 다음에 / 가 한번 더 와야 한다는 의미입니다.
- `$` - "마지막"! 입니다. 그 뒤로 더이상 문자가 오면 안됩니다.

브라우저에 `http://127.0.0.1:8000/post/5/`입력하면, 장고는 `post_detail`인 `view` 를 찾고 있다고 생각하고 pk가 5와 일치한 view 로 정보를 보내게 됩니다..

`pk`는 `primary key`의 약자입니다. 장고 프로젝트에서 자주 사용되는 이름이에요. 물론 여러분은 내가 원하는 변수 이름을 사용할 수 있어요. 예를 들어, `(?P<pk>[0-9]+)`의 변수를 `post_id`바꾼다면 하면 정규표현식도 `(?P<post_id>[0-9]+)`으로 바뀌게 됩니다.

다음 단계는 무엇일까요? 그렇죠. : view를 추가해야죠!

### Post 상세 페이지에 뷰 추가하기
`view` 는 추가적으로 매개변수`pk`를 받아야합니다. view 에 가져다가 써야겠죠? 그래서 함수를 정의할 때, `pk`를 받도록 `def post_detail(request, pk):`라고 정의 할 것입니다. 기입된 `urls(pk)`에 지정한 것과 정확히 똑같은 이름을 사용해야함을 주의하세요. 변수가 생략되면 문제가 생겨 오류가 날 거에요!

쿼리셋(queryset)을 사용해야 합니다.

```
Post.objects.get(pk=pk)
```
하지만 이 코드에는 문제가 있어요. primary key (pk)가있는 Post가 없다면 보고 싶지 않은 오류가 나올 거에요!
![](https://tutorial.djangogirls.org/ko/extend_your_application/images/does_not_exist2.png)

장고에서는 이를 해결 하기위해 특별한 기능을 제공해요. `get_object_or_404`이에요. 이 기능을 사용하면 pk에 맞는 Post가 없을 경우, 멋진 페이지`(페이지 찾을 수 없음 404 : Page Not Found 404)`를 보여줄 거에요.
![](https://tutorial.djangogirls.org/ko/extend_your_application/images/404_2.png)

이제 `views.py` 파일에 `view` 를 추가합시다!

```python
from django.shortcuts import render, get_object_or_404
```
from행 근처로 가서, 파일 맨 끝에 view 를 추가하세요. :

```python
    def post_detail(request, pk):
        post = get_object_or_404(Post, pk=pk)
        return render(request, 'blog/post_detail.html', {'post': post})
```
브라우저를 새로고침 해보세요. : http://127.0.0.1:8000/
![](https://tutorial.djangogirls.org/ko/extend_your_application/images/post_list2.png)

잘 되네요! 그런데 블로그 제목 안의 링크를 클릭하면 어떻게 되나요?
![](https://tutorial.djangogirls.org/ko/extend_your_application/images/template_does_not_exist2.png)

### Post 상세 페이지에 템플릿 만들기
`blog/templates/blog` 디렉토리 안에 `post_detail.html`라는 새 파일을 생성하세요.

```python
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        {% if post.published_date %}
            <div class="date">
                {{ post.published_date }}
            </div>
        {% endif %}
        <h1>{{ post.title }}</h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endblock %}
```

`base.html`을 확장해 봅시다. `content` 블록에서, 블로그 글의 `published_date` 출판일(존재한다면) 과 제목, 내용을 보이게 할 거에요. 그런데 제일 중요한 것을 얘기해봐야하지 않겠어요?

{% if ... %} ... {% endif %} 는 무엇인가를 확인할 때 사용하는 템플릿 태그 입니다. 
페이지를 새로고침하면 페이지 찾을 수 없음(Page not found) 페이지가 없어진 것을 알 수 있어요.

![](https://tutorial.djangogirls.org/ko/extend_your_application/images/post_detail2.png)

## Django 폼

폼(양식, forms)으로 강력한 인터페이스를 만들 수 있어요.
블로그 글을 추가하거나 수정하는 멋진 기능을 추가하는 것이죠. 
장고 폼이 정말 멋진 것은 아무런 준비 없이도 양식을 만들 수 있고, ModelForm을 생성해 자동으로 모델에 결과물을 저장할 수 있다는 거에요.

blog 디렉토리 안에 파일을 만들 거에요.

```
blog
  └── forms.py
```




















usr/local/var/pyenv/versions/가상환경/bin/python3





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
 