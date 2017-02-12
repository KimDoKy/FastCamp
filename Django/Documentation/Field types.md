# Field types


## Model field reference
이 문서는 Django가 제공하는 피드 옵션 및 필드 유형을 포함하여 Field의 모든 API 참조를 포함합니다.

기술적으로, django.db.models.fields에 정의되어 있습니다. 그러나 편의상 django.db.models로 가져옵니다

### Field options
모든 필드 유형에서 사용 가능하고, 모두 선택사항입니다.

#### null
True이면 Django는 빈값을 NULL로 저장합니다. 기본값은 False입니다.
빈 문자열 값은 빈 문자열로 저장되므로 CharField 및 TextField와 같은 문자열 기반 필드에서는 null을 사용하지 마십시오.
문자열,비문자열 기반의 필드는 null 매개변수가 데이터베이스 저장소에만 영향을  주기 때문에, 빈 값을 허용하려면 **blank = True**로 설정해야합니다

-
#### blank
True이면 필드를 비워 둘 수 있습니다. 기본값은 False입니다.

null은 데이터베이스과 관련 있지만 blank는 유효성 검사과 관련있습니다.
필드에 blank=True이면 양식 유효성 검사에서 빈값을 입력할 수 있습니다.

-
#### choices
두 항목씩 구성된  튜플 형식([(A, B), (A, B) ...])으로 choice 필드 옵션을 주면, 기본 양식 위젯은 표준 텍스트 필드 대신 이러한 선택 항목이있는 선택 상자를 보여줍니다.

```python
from django.db import models

class Student(models.Model):
    FRESHMAN = 'FR'
    SOPHOMORE = 'SO'
    JUNIOR = 'JR'
    SENIOR = 'SR'
    YEAR_IN_SCHOOL_CHOICES = (
        (FRESHMAN, 'Freshman'),
        (SOPHOMORE, 'Sophomore'),
        (JUNIOR, 'Junior'),
        (SENIOR, 'Senior'),
    )
    year_in_school = models.CharField(
        max_length=2,
        choices=YEAR_IN_SCHOOL_CHOICES,
        default=FRESHMAN,
    )

    def is_upperclass(self):
        return self.year_in_school in (self.JUNIOR, self.SENIOR)
```
표시 값은 `get_FOO_display()` 메소드를 사용하여 액세스 할 수 있습니다.
반드시 목록이나 튜플 일 필요는 없습니다. 이를 통해 선택 사항을 동적으로 구성 할 수 있습니다.
`blank = False`가 기본값과 함께 필드에 설정되어 있지 않으면 `"---------"`가 포함 된 레이블이 선택 상자와 함께 렌더링됩니다. 

-
#### db_column
필드에서 사용할 데이터베이스의 컬럼 이름입니다.
만약 지정하지 않는다면 Django는 필드의 이름으로 사용합니다.

-
#### db_index
설정이 True이면 필드에 대해 데이터베이스 index가 작성됩니다.

-
#### db_tablespace
이 필드의  index가 작성된 경우, index에 사용할 데이터베이스 테이블 공간의 이름입니다.

-
#### default
필드의 기본값입니다. 값 또는 호출이 가능한 객체일 수 있습니다. 호출이 가능하면 새로운 객체가 생성될때마다 호출됩니다.
기본값은 변경이 불가능합니다.

```python
def contact_default():
    return {"email": "to1@example.com"}

contact_info = JSONField("ContactInfo", default=contact_default)
```

-
#### editable
False 인 경우 필드는 관리자 또는 다른 ModelForm에 표시되지 않습니다. 또한 모델 유효성 검사 도중 건너 뜁니다. 기본값은 True입니다.

-
#### error_messages
필드에서 발생시키는 오류 기본 메시지를 대체 할 수 있습니다.

-
#### help_text
폼 위젝 도움말로 표시됩니다.
help_text에 HTML을 포함시킬 수도 있습니다.

```python
help_text="Please use the following format: <em>YYYY-MM-DD</em>."
```

-
#### primary_key
True이면이 필드는 모델의 기본 키입니다.
`primary_key = True`는 `null = False` 및 `unique = True`를 의미합니다. 하나의 기본 키만 객체에 허용됩니다.
primary_key는 읽기 전용입니다. primary_key 값을 변경하고 저장하면 이전 개체와 함께 새 개체가 생성됩니다.

-
#### unique
True이면 해당 필드 값은 해당 테이블에서 유일한 값을 가져야 합니다.
이는 데이터베이스 레벨 및 모델 검증에 의해 시행됩니다. 중복값이 있는 모델을 저장하려고 하면 `save()`메소드에 의해 오류가 발생합니다.(django.db.IntegrityError)
이 옵션은 ManyToManyField, OneToOneField 및 FileField를 제외한 모든 필드 유형에 유효합니다.

-
#### unique_for_date
DateField 또는 DateTimeField의 이름으로 설정하여이 필드를 날짜 필드의 값에 대해 고유해야합니다.
예를 들어, `unique_for_date = "pub_date '`필드 제목이 있다면, Django는 동일한 제목과 pub_date을 가진 두 레코드의 입력을 허용하지 않습니다.

-
#### unique_for_month
unique_for_date와 같지만 월을 기준으로 필드가 고유해야합니다.

-
#### unique_for_year
unique_for_date와 같지만 년(해)을 기준으로 필드가 고유해야합니다.

-
#### verbose_name
필드 이름을 손쉽게 페이지에서 출력시켜줍니다.
필드들은 기본적으로 첫번째 인자로 verbose name 값을 받습니다.

-
#### validators
유효성을 검사합니다.
[참조 . validators documentation](https://docs.djangoproject.com/en/1.10/ref/validators/)

##### Registering and fetching lookups
API를 사용하여 필드 클래스에 사용할 수있는 조회와 필드에서 조회를 가져 오는 방법을 사용자 정의 할 수 있습니다.

-
### Field types

#### AutoField
class AutoField(**options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#AutoField)
사용 가능한 ID에 따라 자동으로 증가하는 IntegerField입니다.
보통 직접 사용 할 필요는 없습니다.

-
#### BigAutoField
class BigIntegerField(**options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#BigIntegerField)

AutoField와 동일하고 64-bit(1 to 9223372036854775807)까지 보장합니다.

-
#### BigIntegerField
class BigIntegerField(**options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#BigIntegerField)

BigAutoField와 동일하고 64-bit정수(-9223372036854775808 to 9223372036854775807.)까지 보장합니다.

-
#### BinaryField
class BinaryField(**options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#BinaryField)

원시 이진 데이터를 저장하는 필드입니다. 바이트 할당 만 지원합니다. 이 입력란에는 기능이 제한되어 있습니다. 예를 들어, BinaryField 값에 대한 쿼리 집합을 필터링 할 수 없습니다. 또한 BinaryField를 ModelForm에 포함시키는 것도 불가능합니다.

-
#### BooleanField
class BooleanField(**options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#BooleanField)

기본 양식 위젯은 CheckboxInput입니다. 
null 값을 받아 들일 필요가 있다면 대신 `NullBooleanField`를 사용하십시오. `Field.default`가 정의되어 있지 않으면 BooleanField의 기본값은 None입니다.

-
#### CharField
class CharField(max_length=None, **options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#CharField)

적은 량의 문자열을 나타내는 문자열필드입니다.
많은 양의 텍스트는 TextField를 사용해야 합니다.
이 필드의 기본 양식 위젯은 TextInput입니다. 
>CharField.max_length  
필드의 최대 문자수입니다. 유효성 검사에 적용할 수 있습니다.

-
#### CommaSeparatedIntegerField
class CommaSeparatedIntegerField(max_length=None, **options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#CommaSeparatedIntegerField)

쉼표로 구분 된 정수 필드. CharField와 마찬가지로 max_length 인수가 필요합니다.

-
#### DateField
class DateField(auto_now=False, auto_now_add=False, **options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#DateField)

Python에서 datetime.date 인스턴스로 표현되는 날짜입니다.
몇가지 선택적 인수가 있습니다.
>
DateField.auto_now
개체가 저장 될 때마다 지금 필드를 자동으로 설정합니다.
"마지막으로 수정된 시간"으로 사용할 때 유용합니다.
재정의 할 수 없습니다.
이 필드는 Model.save ()를 호출 할 때 자동으로 업데이트됩니다.
>
DateField.auto_now_add
객체가 처음 생성 될 때 자동으로 필드를 지금으로 설정합니다.
재정의 할 수 없습니다. 따라서 객체를 만들 때이 필드의 값을 설정하더라도 무시됩니다. 이 필드의 기본 양식 위젯은 TextInput입니다. 관리자는 JavaScript 캘린더와 "오늘"에 대한 단축키를 추가합니다. 
>
auto_now_add, auto_now 및 default 옵션은 함께 사용할 수 없습니다. 이러한 옵션을 조합하면 오류가 발생합니다.

>> auto_now 또는 auto_now_add를 True로 설정하면 해당 필드는 editable = False 및 blank = True로 설정됩니다.

-
#### DateTimeField
class DateTimeField(auto_now=False, auto_now_add=False, **options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#DateTimeField)

Python에서 datetime.datetime 인스턴스로 표현되는 날짜와 시간입니다.

-
#### DecimalField
class DecimalField(max_digits=None, decimal_places=None, **options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#DecimalField)

고정 소수점이하의 십진수로 파이썬에서 decimal 인스턴스로 나타냅니다.
두가지 필수 인수가 있습니다.
>
DecimalField.max_digits
숫자에 허용되는 최대 자릿수입니다. 이 수는 decimal_places보다 크거나 같아야합니다.
>
DecimalField.decimal_places
숫자와 함께 저장할 소수점 이하 자릿수

예를 들어, 최대 999 자리를 소수점 이하 2 자리로 저장하려면 다음을 사용하십시오.

```python
models.DecimalField(..., max_digits=5, decimal_places=2)
```

-
#### DurationField
class DurationField(**options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#DurationField)

Timedelta가 Python으로 모델링한 **시간주기 저장 필드**입니다.
>DurationField를 사용한 산술은 대부분의 경우에 작동합니다. 그러나 PostgreSQL 이외의 모든 데이터베이스에서 DurationField 값을 DateTimeField의 산술 인스턴스와 비교하는 것은 예상대로 작동하지 않습니다.

-
#### EmailField
class EmailField(max_length=254, **options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#EmailField)

값이 유효한 전자 메일 주소인지 확인하는 CharField입니다.

-
#### FileField
class FileField(upload_to=None, max_length=100, **options)[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/files/#FileField)

**파일 업로드 필드**입니다.
>primary_key 및 unique 인수는 지원되지 않으며 사용되는 경우 TypeError를 발생시킵니다.

2개의 선택적 인수가 있습니다.
> **FileField.upload_to**
>
업로드 디렉토리와 파일 이름을 설정하는 방법을 제공하며 두 가지 방법으로 설정할 수 있습니다. 두 경우 모두 값은 Storage.save () 메서드에 전달됩니다.
>
```python
class MyModel(models.Model):
    # file will be uploaded to MEDIA_ROOT/uploads
    upload = models.FileField(upload_to='uploads/')
    # or...
    # file will be saved to MEDIA_ROOT/uploads/2015/01/30
    upload = models.FileField(upload_to='uploads/%Y/%m/%d/')
```
>기본 FileSystemStorage를 사용하는 경우 문자열 값은 MEDIA_ROOT 경로에 추가되어 업로드 된 파일이 저장 될 로컬 파일 시스템의 위치를 ​​형성합니다.
>
upload_to는 함수와 같은 호출 가능 함수 일 수도 있습니다. 이것은 파일 이름을 포함하여 업로드 경로를 얻기 위해 호출됩니다. 이 호출 가능 객체는 두 개의 인수를 받아 들여 스토리지 시스템에 전달되는 유닉스 스타일 경로 (슬래시 포함)를 반환해야합니다.
>
Argument | Description
---|---
instance | FileField가 정의 된 모델의 인스턴스입니다.<br> 대부분의 경우이 개체는 아직 데이터베이스에 저장되지 않았으므로 기본 자동 필드를 사용하는 경우 기본 키 필드의 값이 아직 없을 수 있습니다.
filename | 원래 파일에 주어진 파일 이름
>
```
def user_directory_path(instance, filename):
    # file will be uploaded to MEDIA_ROOT/user_<id>/<filename>
    return 'user_{0}/{1}'.format(instance.user.id, filename)
class MyModel(models.Model):
    upload = models.FileField(upload_to=user_directory_path)
```
>
> **FileField.storage**
>
파일의 저장 및 검색을 처리하는 저장 개체입니다.
>
1. 설정 파일에서 Django가 업로드 된 파일을 저장할 디렉토리의 전체 경로로 MEDIA_ROOT을 정의해야합니다. 성능을 위해 이러한 파일은 데이터베이스에 저장되지 않습니다.
2. 모델에 FileField 또는 ImageField를 추가하고 upload_to 옵션을 정의하여 업로드 된 파일에 사용할 MEDIA_ROOT의 하위 디렉토리를 지정합니다.
3. 데이터베이스에 저장되는 것은 모두 파일에 대한 경로입니다 (MEDIA_ROOT에 상대적 임).
Django가 제공하는 편의 url 속성을 사용하고 싶을 것입니다. 예를 들어 ImageField의 이름이 mug_shot 인 경우 {{object.mug_shot.url}} 템플릿을 사용하여 이미지의 절대 경로를 가져올 수 있습니다.
>
업로드 된 파일의 디스크상의 파일 이름이나 파일의 크기를 검색하려면 name 및 size 속성을 각각 사용할 수 있습니다.
>
업로드된 파일을 다룰 때 업로드한 파일과 파일의 유형에 주의를 기울여야 합니다.(보안상의 문제입니다.) 누군가 CGI나 PHP 스크립트를 업로드하고 사이트를 방문하여 해당 스크립트를 실행할 수도 있습니다.
>
다른 필드와 마찬가지로 max_length 인수를 사용하여 최대 길이를 변경할 수 있습니다.

##### FileField and FieldFile
class FieldFile[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/files/#FieldFile)

모델에서 FileField에 액세스하면 FieldFile 인스턴스가 기본 파일에 액세스하기위한 프록시로 제공됩니다.
FieldFile의 API는 File의 API와 매우 다른 점이 하나 있습니다. 클래스에 의해 래핑 된 객체는 반드시 파이썬의 내장 파일 객체를 감싸는 래퍼 일 필요는 없습니다. 대신 Storage.open () 메서드의 결과를 둘러싼 래퍼입니다.

read () 및 write ()와 같이 File에서 상속 된 API 외에도 FieldFile에는 기본 파일과 상호 작용하는 데 사용할 수있는 몇 가지 메서드가 포함되어 있습니다.
>save ()와 delete ()는 기본적으로 연관된 FieldFile의 모델 객체를 데이터베이스에 저장합니다.

- FieldFile.name
관련 FileField의 저장소 루트에서 상대 경로를 포함한 파일의 이름입니다.
- FieldFile.size
기본 Storage.size () 메서드의 결과입니다.
- FieldFile.url
기본 Storage 클래스의 url () 메서드를 호출하여 파일의 상대 URL에 액세스하는 읽기 전용 속성입니다.
- FieldFile.open(mode='rb')
지정된 모드에서 인스턴스와 관련된 파일을 엽니다. 파이썬 open () 메소드와는 달리, 파일 디스크립터를 리턴하지 않습니다.
- FieldFile.close()
파이썬 file.close () 메소드와 유사하게 동작하고이 인스턴스와 관련된 파일을 닫습니다.
- FieldFile.save(name, content, save=True)
파일 이름과 파일 내용을 가져 와서 필드의 저장소 클래스에 전달한 다음 저장된 파일을 모델 필드와 연결합니다.
두 개의 필수 인수를 취합니다.  
**name**은 파일의 이름이고 **content**는 파일 내용을 포함하는 객체입니다. 선택적 **save** 인수는이 필드와 연관된 파일이 변경된 후에 모델 인스턴스가 저장되는지 여부를 제어합니다. 기본값은 True입니다.
**content** 인수는 Python의 내장 파일 객체가 아닌 django.core.files.File의 인스턴스 여야합니다.

```python
from django.core.files import File
# Open an existing file using Python's built-in open()
f = open('/path/to/hello.world')
myfile = File(f)
```
```python
from django.core.files.base import ContentFile
myfile = ContentFile("hello world")
```

- FieldFile.delete(save=True)
이 인스턴스와 관련된 파일을 삭제하고 필드의 모든 특성을 삭제합니다.  
선택적 save 인수는이 필드와 연관된 파일이 삭제 된 후에 모델 인스턴스가 저장되는지 여부를 제어합니다. 기본값은 True입니다.
모델을 삭제하면 관련 파일이 삭제되지 않습니다.

-
##### FilePathField
>
**`class FilePathField(path=None, match=None, recursive=False, max_length=100, **options)`**[[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#FilePathField)]

CharField는 파일 시스템의 특정 디렉토리에있는 파일 이름으로 제한됩니다

- FilePathField.path
필수 사항. 이 FilePathField가 선택해야하는 디렉토리에 대한 절대 파일 시스템 경로.
- FilePathField.match
선택 사항. FilePathField가 파일 이름을 필터링하는 데 사용할 정규 표현식입니다. 정규 표현식은 전체 경로가 아닌 기본 파일 이름에 적용됩니다.
- FilePathField.recursive
선택 사항. 참 또는 거짓. 기본값은 False입니다. path의 모든 하위 디렉토리가 포함되어야하는지 여부를 지정합니다.
- FilePathField.allow_files
선택 사항. 참 또는 거짓. 기본값은 참입니다. 지정된 위치의 파일을 포함할지 여부를 지정합니다. this 또는 allow_folders는 True 여야합니다.
- FilePathField.allow_folders
선택 사항. 참 또는 거짓. 기본값은 False입니다. 지정된 위치의 폴더를 포함할지 여부를 지정합니다. this 또는 allow_files는 True 여야합니다.

```
FilePathField(path="/home/images", match="foo.*", recursive=True)
```
FilePathField 인스턴스는 기본 최대 길이가 100자인 varchar 열로 데이터베이스에 만들어집니다. 다른 필드와 마찬가지로 max_length 인수를 사용하여 최대 길이를 변경할 수 있습니다.

-
#### FloatField
**`class FloatField(**options)`**[[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#FloatField)]

Python으로 표현 된 부동 소수점 숫자입니다.

-
#### ImageField
`**class ImageField(upload_to=None, height_field=None, width_field=None, max_length=100, **options)**`[[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/files/#ImageField)]

업로드 된 개체가 유효한 이미지인지 확인합니다. FileField에서 사용할 수있는 특수 특성 외에도 ImageField에는 height 및 width 특성이 있습니다.

두 개의 추가 선택적 인수가 있습니다.

- ImageField.height_field
모델 인스턴스가 저장 될 때마다 이미지 높이로 자동 채워지는 모델 필드의 이름입니다.
- ImageField.width_field
모델 인스턴스가 저장 될 때마다 이미지 너비가 자동으로 채워지는 모델 필드의 이름입니다.

이 필드에 대한 기본 양식 위젯은 ClearableFileInput입니다.

-
#### IntegerField
**`class IntegerField(**options)`**[[source](https://docs.djangoproject.com/en/1.10/_modules/django/db/models/fields/#IntegerField)]

정수. -2147483648에서 2147483647까지의 값은 장고가 지원하는 모든 데이터베이스에서 안전합니다.

-
#### GenericIPAddressField
**`class GenericIPAddressField(protocol='both', unpack_ipv4=False, **options)`**[[source]

문자열 형식의 IPv4 또는 IPv6 주소 (예 : 192.0.2.30 또는 2a02 : 42fe :: 4)

- GenericIPAddressField.protocol
지정된 프로토콜에 대한 유효한 입력을 제한합니다. 허용되는 값은 'both'(기본값), 'IPv4'또는 'IPv6'입니다.
- GenericIPAddressField.unpack_ipv4
IPv4 매핑 된 주소의 압축을 풉니 다.

-
#### NullBooleanField
**`class NullBooleanField(**options)`**

BooleanField와 같으나 옵션 중 하나로 NULL을 허용합니다.

-
#### PositiveIntegerField
**`class PositiveIntegerField(**options)`**

IntegerField와 같지만 양수 또는 0이어야합니다.

-
#### PositiveSmallIntegerField
**`class PositiveSmallIntegerField(**options)`**

PositiveIntegerField와 같지만 특정 (데이터베이스 종속적 인) 지점에서만 값을 허용합니다.

-
#### SlugField
**`class SlugField(max_length=50, **options)`**

슬러그는 글자, 숫자, 밑줄 또는 하이픈 만 포함하는 짧은 레이블입니다. 일반적으로 URL에 사용됩니다.

-
#### SmallIntegerField
**`class SmallIntegerField(**options)`**

IntegerField와 같지만 특정 (데이터베이스 종속적 인) 지점에서만 값을 허용합니다.

-
#### TextField
**`class TextField(**options)`**

대량의 텍스트 필드입니다.

-
#### TimeField
**`class TimeField(auto_now=False, auto_now_add=False, **options)`**

Python에서 datetime.time 인스턴스로 표현 된 시간입니다. DateField와 동일한 자동 채우기 옵션을 적용합니다.

-
#### URLField
**`class URLField(max_length=200, **options)`**

URL의 CharField입니다.
이 필드의 기본 양식 위젯은 TextInput입니다.

-
#### UUIDField
**`class UUIDField(**options)`**

유일한 식별자를 저장하기위한 필드입니다.
파이썬의 UUID 클래스를 사용합니다.

```python
import uuid
from django.db import models

class MyUUIDModel(models.Model):
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    # other fields
```

- 
### Relationship fields
Django는 또한 관계를 나타내는 일련의 필드를 정의합니다.

#### ForeignKey
**`class ForeignKey(othermodel, on_delete, **options)`**

many-to-one 관계. 위치 인수가 필요합니다. 모델이 관련 지을 수 있고있는 클래스입니다.

재귀 관계 (자체와 다 대일 관계가있는 객체)를 만들려면 models.ForeignKey ( 'self', on_delete = models.CASCADE)를 사용합니다.

```pythom
from django.db import models

class Car(models.Model):
    manufacturer = models.ForeignKey(
        'Manufacturer',
        on_delete=models.CASCADE,
    )
    # ...

class Manufacturer(models.Model):
    # ...
    pass
```
```python
# products/models.py
from django.db import models

class AbstractCar(models.Model):
    manufacturer = models.ForeignKey('Manufacturer', on_delete=models.CASCADE)

    class Meta:
        abstract = True		# 추상 클래스 지정
```
```python
# production/models.py
from django.db import models
from products.models import AbstractCar

class Manufacturer(models.Model):
    pass

class Car(AbstractCar):
    pass

# Car.manufacturer will point to `production.Manufacturer` here.
```
```python
# 다른 app에 정의 된 모델을 참조하려면 전체app레이블로 모델을 명시 적으로 지정할 수 있습니다.
class Car(models.Model):
    manufacturer = models.ForeignKey(
        'production.Manufacturer',
        on_delete=models.CASCADE,
    )
```
데이터베이스 인덱스는 ForeignKey에 자동으로 작성됩니다.

##### Database Representation
Django는 "_id"를 필드 이름에 추가하여 데이터베이스 열 이름을 만듭니다.

-
##### Arguments
ForeignKey는 관계가 작동하는 방식에 대한 세부 정보를 정의하는 다른 인수를 허용합니다.
>
**ForeignKey.on_delete**
ForeignKey가 참조하는 객체가 삭제되면 Django는 on_delete 인수에 의해 지정된 SQL 제약 조건의 동작을 에뮬레이션합니다.

```python
# nullable ForeignKey가 있고 참조 된 객체가 삭제 될 때 null로 설정되기를 원할 경우
user = models.ForeignKey(
    User,
    models.SET_NULL,
    blank=True,
    null=True,
)
```
on_delete에 가능한 값은 django.db.models에 있습니다.
- CASCADE : 계단식 삭제.  ForeignKey가 포함 된 객체도 삭제합니다.
- PROTECT : ProtectedError를 발생시켜 참조 된 객체의 삭제를 방지합니다.
- SET_NULL : ForeignKey null을 설정. null가 True 인 경우에만 가능합니다.
- SET_DEFAULT : ForeignKey를 기본값으로 설정. ForeignKey의 기본값을 설정해야합니다.
- SET() : `ForeignKey를 SET ()`에 전달 된 값으로 설정하거나 호출 가능 객체가 전달 된 경우 호출 한 결과로 설정합니다.

```python
from django.conf import settings
from django.contrib.auth import get_user_model
from django.db import models

def get_sentinel_user():
    return get_user_model().objects.get_or_create(username='deleted')[0]

class MyModel(models.Model):
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.SET(get_sentinel_user),
    )
```
- DO_NOTHING : 데이터베이스 백엔드가 참조 무결성을 적용하면 데이터베이스 필드에 SQL ON DELETE 제약 조건을 수동으로 추가하지 않으면 IntegrityError가 발생합니다.

**ForeignKey.limit_choices_to**
ModelForm 또는 admin을 사용하여 필드를 렌더링 할 때, 필드에 사용할 수있는 선택 항목에 대한 제한을 설정합니다.

```python
staff_member = models.ForeignKey(
    User,
    on_delete=models.CASCADE,
    limit_choices_to={'is_staff': True},
)
```
ModelForm의 해당 필드가 `is_staff = True` 인 사용자 만 나열하도록합니다.

예를 들어 Python datetime 모듈과 함께 사용하여 날짜 범위에 따라 선택을 제한하는 경우 호출 가능 형식이 유용 할 수 있습니다

```python
def limit_pub_date_choices():
    return {'pub_date__lte': datetime.date.utcnow()}

limit_choices_to = limit_pub_date_choices
```
**ForeignKey.related_name**
관련 객체에서 이 객체에 대한 관계에 사용할 이름입니다. 
또한 related_query_name (대상 모델의 역 필터 이름에 사용할 이름)의 기본값입니다. 추상 모델에 관계를 정의 할 때이 값을 설정해야합니다. 그렇게하면 특별한 구문을 사용할 수 있습니다.

Django가 역방향 관계를 만들지 않기를 원한다면, `related_name`을 '+'로 설정하거나 '+'로 끝내십시오. 예를 들어, 이렇게하면 User 모델이이 모델에 대한 역방향 관계를 갖지 않게됩니다.

```python
user = models.ForeignKey(
    User,
    on_delete=models.CASCADE,
    related_name='+',
)
```

**ForeignKey.related_query_name**
대상 모델에서 역방향 필터 이름에 사용할 이름입니다. 

```python
# Declare the ForeignKey with related_query_name
class Tag(models.Model):
    article = models.ForeignKey(
        Article,
        on_delete=models.CASCADE,
        related_name="tags",
        related_query_name="tag",
    )
    name = models.CharField(max_length=255)

# That's now the name of the reverse filter
Article.objects.filter(tag__name="important")
```

**ForeignKey.to_field**
관계가 있는 관련 객체의 필드입니다.

**ForeignKey.db_constraint**
외래 키에 대해 데이터베이스에 제약 조건을 만들지 여부를 제어합니다. 기본값은 True이며, 이것을 False로 설정하면 데이터 무결성이 매우 나쁠 수 있습니다.

**ForeignKey.swappable**
ForeignKey가 스왑 가능 모델을 가리키는 경우 마이그레이션 프레임 워크의 반응을 제어합니다.

-
#### ManyToManyField
> **`class ManyToManyField(othermodel, **options)`**

many-to-many 관계. 위치 지정 인수가 필요합니다. 모형이 관련된 클래스이며, 재귀 및 지연 관계를 포함하여 ForeignKey에서와 똑같이 작동합니다. 관련 객체는 필드의 RelatedManager를 사용하여 추가, 제거 또는 생성 할 수 있습니다.

##### Database Representation
Django는 many-to-many 관계를 표현하기 위해 **중간 테이블**을 생성합니다. 기본적으로이 테이블 이름은 many-to-many 필드의 이름과 그 테이블을 포함하는 모델의 테이블 이름을 사용하여 생성됩니다.   db_table 옵션을 사용하여 조인 테이블의 이름을 수동으로 제공 할 수 있습니다.

-
##### Arguments


#### OneToOneField

### Field API reference

## Field attribute reference
### Attributes for fields
### Attributes for fields with relations