# QuerySet method reference

## QuerySet API reference
### When QuerySets are evaluated
내부적으로 QuerySet은 실제로 데이터베이스에 도달하지 않고 구성, 필터링, 슬라이스 및 일반적으로 전달 될 수 있습니다. 쿼리 세트를 평가할 때까지 실제로 데이터베이스 활동이 발생하지 않습니다.

다음과 같은 방법으로 QuerySet을 평가할 수 있습니다.

- Iteration :   
QuerySet은 반복 가능하며 처음 반복 할 때 QuerySet을 실행합니다.

```python
#  데이터베이스의 모든 항목의 제목을 인쇄합니다.
for e in Entry.objects.all():
    print(e.headline)
```
- Slicing :  
QuerySet은 Python의 배열 슬라이싱 구문을 사용하여 슬라이스 할 수 있습니다.  
또한 평가되지 않은 QuerySet을 슬라이스하는 경우 평가되지 않은 또 다른 QuerySet이 반환되지만 더 많은 필터를 추가하거나 순서를 수정하는 것은 허용되지 않습니다. 이는 SQL로 잘 변환되지 않으며 명확한 의미도 가지지 않기 때문입니다.

- Pickling/Caching :  
Pickling QuerySets 참조.  
중요한 점은 결과가 데이터베이스에서 읽혀집니다.

- repr() :  
QuerySet은 `repr ()`을 호출 할 때 평가됩니다. 이것은 Python 인터프리터 인터프리터의 편의를 위해 제공되므로 대화식으로 API를 사용할 때 결과를 즉시 볼 수 있습니다.

- len() :  
QuerySet은 `len ()`을 호출 할 때 평가됩니다. 예상 한대로 결과 목록의 길이를 반환합니다.

- list() :  
QuerySet의 `list ()`를 호출하여 QuerySet을 강제 평가합니다.

```python
entry_list = list(Entry.objects.all())
```

- bool() :  
부울 컨텍스트에서 QuerySet을 테스트하면 쿼리가 실행됩니다. 적어도 하나의 결과가 있으면 QuerySet은 True이고, 그렇지 않으면 False입니다. 

```python
if Entry.objects.filter(headline="Test"):
   print("There is at least one Entry with the headline Test")
```

#### Pickling QuerySets
QuerySet을 pickle하면 pickling 전에 메모리에 모든 결과가 로드됩니다. pickling은 일반적으로 캐싱의 선구자로 사용되며 캐싱 된 쿼리 세트가 다시 로드 될 때 결과가 이미 사용되고 사용할 준비가 되기를 원합니다.  
(데이터베이스 읽기는 캐싱의 목적을 무너뜨리기까지 어느 정도 시간이 걸릴 수 있습니다.) 즉, 쿼리 집합을 실행 취소 할 때 현재 데이터베이스에 있는 결과가 아닌 쿼리 된 결과가 포함됩니다.

나중에 데이터베이스에서 QuerySet을 재 작성하기 위해 필요한 정보만을 pickle 경우 QuerySet의 쿼리 속성을 선택하십시오.

```
# 원래의 QuerySet을 다시로드 할 수 있습니다 (결과가로드되지 않음)
>>> import pickle
>>> query = pickle.loads(s)     # Assuming 's' is the pickled string.
>>> qs = MyModel.objects.all()
>>> qs.query = query            # Restore the original 'query'.
```
query 속성은 불투명 한 객체입니다. 이는 쿼리 작성의 내부를 나타내며 공개 API의 일부가 아닙니다. 
> 버전 간의 호환이 되지 않습니다.  
> 자동으로 손상된 객체와 같이 pickle 호환성 오류를 진단하기가 어려울 수 있기 때문에 Python과 다른 Django 버전의 쿼리 세트를 unpickle하려고하면 `RuntimeWarning`이 발생합니다.

-
### QuerySet API
QuerySet의 공식 선언입니다.
> **`class QuerySet(model=None, query=None, using=None)`**

일반적으로 QuerySet과 상호 작용할 때 filter를 연결하여 사용합니다. 이 작업을 수행하기 위해 대부분의 QuerySet 메소드는 새로운 쿼리 세트를 반환합니다.

QuerySet 클래스에는 인트로 스펙트에 사용할 수있는 두 개의 공용 속성이 있습니다.

- ordered :  
QuerySet이 순서가 맞으면 true입니다.
- db :  
이 쿼리가 지금 실행될 경우 사용할 데이터베이스입니다.

#### Methods that return new QuerySets
Django는 QuerySet에 의해 리턴 된 결과의 타입이나 SQL 쿼리가 실행되는 방식을 변경하는 다양한 QuerySet 메소드를 제공합니다.
##### filter()
**`filter(**kwargs)`**  
지정된 검색 매개 변수와 **일치하는 객체**가 포함 된 새 QuerySet을 반환합니다. 여러 매개 변수는 기본 SQL 문에서 AND를 통해 조인됩니다. 더 복잡한 쿼리 (예 : OR 문을 사용하는 쿼리)를 실행해야하는 경우 Q 개체를 사용할 수 있습니다.

-
##### exclude()
**`exclude(**kwargs)`**  
지정된 조회 매개 변수와 **일치하지 않는 객체**가 포함 된 새 QuerySet을 반환합니다.  여러 매개 변수는 기본 SQL 문에서 AND를 통해 조인되며 모든 것은 `NOT ()`으로 묶입니다. 

```python
#pub_date가 2005-1-3보다 늦고 제목이 "Hello"인 모든 항목을 제외합니다.  

Entry.objects.exclude(pub_date__gt=datetime.date(2005, 1, 3), headline='Hello')
```
SQL으로 다음과 같이 평가됩니다.

```sql
SELECT ...
WHERE NOT (pub_date > '2005-1-3' AND headline = 'Hello')
```
```python
#  pub_date가 2005-1-3보다 늦거나 모든 제목이 "Hello"인 모든 항목을 제외합니다.

Entry.objects.exclude(pub_date__gt=datetime.date(2005, 1, 3)).exclude(headline='Hello')
```
SQL으로 다음과 같이 평가됩니다.

```sql
SELECT ...
WHERE NOT pub_date > '2005-1-3'
AND NOT headline = 'Hello'
```

-
##### annotate()
**`annotate(*args, **kwargs)`**  
제공된 쿼리 식 목록으로 QuerySet의 각 개체에 주석을 첨부합니다.
`annotate ()`에 대한 각 인수는 반환되는 QuerySet의 각 객체에 추가되는 주석입니다.

키워드 인수를 사용하여 지정된 주석은이 키워드를 주석의 별칭으로 사용합니다. 익명 인수에는 집계 함수의 이름과 집계중인 모델 필드를 기반으로 별칭이 생성됩니다. 단일 필드를 참조하는 집계 식만 익명 인수가 될 수 있습니다. 다른 모든 것은 키워드 인수 여야합니다.

```python
# 블로그 목록을 조작하는 경우 각 블로그에서 몇 개의 항목이 작성되었는지 확인할 수 있습니다.

>>> from django.db.models import Count
>>> q = Blog.objects.annotate(Count('entry'))
# The name of the first blog
>>> q[0].name
'Blogasaurus'
# The number of entries on the first blog
>>> q[0].entry__count
42
```
```python
# 블로그 모델은 entry__count 속성 자체를 정의하지 않지만 키워드 인수를 사용하여 집계 함수를 지정하면 주석의 이름을 제어 할 수 있습니다.

>>> q = Blog.objects.annotate(number_of_entries=Count('entry'))
# The number of entries on the first blog, using the name provided
>>> q[0].number_of_entries
42
```

-
##### order_by()
**`order_by(*fields)`**  
기본적으로 QuerySet에 의해 반환 된 결과는 모델 메타의 정렬 옵션에 의해 주어진 순서 튜플에 의해 정렬됩니다. `order_by` 메소드를 사용하여 QuerySet 단위로이 값을 겹쳐 쓸 수 있습니다.

```python
# pub_date가 내림차순으로 정렬 된 다음 제목이 오름차순으로 정렬됩니다. "-pub_date"앞에있는 음 기호는 내림차순을 나타냅니다. 오름차순이 내포되어 있습니다. 

Entry.objects.filter(pub_date__year=2005).order_by('-pub_date', 'headline')
```
```python
#  무작위로 주문하려면 다음과 같이 "?"를 사용하십시오.

Entry.objects.order_by('?')
```
> 참고 : `order_by ( '?')` 쿼리는 사용하는 데이터베이스 백엔드에 따라 값이 비싸고 느릴 수 있습니다.

다른 모델의 필드로 정렬하려면 모델 관계를 쿼리 할 때와 같은 구문을 사용하십시오. 즉, 필드 이름과 이중 밑줄 `(__)`, 새 모델의 필드 이름 등이 포함됩니다. 원하는 모델을 추가 할 수 있습니다.

```python
Entry.objects.order_by('blog__name', 'headline')
```
Django는 다른 모델과 관계가있는 필드로 정렬을 시도하면 관련 모델의 기본 순서를 사용하거나 Meta.ordering이 지정되지 않은 경우 관련 모델의 기본 키순으로 정렬합니다. 

```python
Entry.objects.order_by('blog')
# 서로 같은 결과가 나옵니다.
Entry.objects.order_by('blog__id')
```
```
# Blog가 ordering = [ 'name'] 인 경우

Entry.objects.order_by('blog__name')
```
관련 필드의 `_id`를 참조하여 JOIN의 비용을 들이지 않고 관련 필드에서 쿼리 세트를 주문할 수도 있습니다.

```
# No Join
Entry.objects.order_by('blog_id')

# Join
Entry.objects.order_by('blog__id')
```

표현식에서 `asc ()` 또는 `desc ()`를 호출하여 쿼리 식으로 정렬 할 수도 있습니다

```
Entry.objects.order_by(Coalesce('summary', 'headline').desc())
```
>
결과를 정렬하기 위해 다중 값 필드를 지정할 수 있습니다.
>
```python
class Event(Model):
   parent = models.ForeignKey(
       'self',
       on_delete=models.CASCADE,
       related_name='children',
   )
   date = models.DateField()
Event.objects.order_by('children__date')
```
>
여기에는 잠재적으로 각 이벤트에 대한 여러 주문 데이터가 있을 수 있습니다. 여러 자식이 있는 각 이벤트는 `order_by ()`가 생성한 새QuerySet으로 여러번 반환됩니다. 즉, QuerySet에서 `order_by ()`를 사용하면 예상했던 것보다 더 많은 항목을 반환 할 수 있습니다.
따라서 다중 값 필드를 사용하여 결과를 정렬 할 때는 주의해야합니다. 주문하는 품목마다 주문 데이터가 하나만 있다는 것을 확신 할 수 있다면 이 접근법은 문제를 나타내지 않아야합니다. 그렇지 않은 경우 결과가 예상 한 결과인지 확인하십시오.

순서가 대소 문자를 구분해야하는지 여부를 지정할 방법이 없습니다.

```python
# 대소 문자가 일치하는 순서를 얻을 수 있도록 Lower로 소문자로 변환 된 필드로 정렬 할 수 있습니다.

Entry.objects.order_by(Lower('headline').desc())
```
기본 순서가 아니더라도 쿼리에 적용 할 순서를 원하지 않으면 매개 변수없이 `order_by ()`를 호출하십시오.

각 `order_by ()` 호출은 이전의 모든 순서를 지웁니다. 

```python
# 검색어는 제목이 아닌 pub_date에 의해 정렬됩니다.

Entry.objects.order_by('headline').order_by('pub_date')
```

##### reverse()
**`reverse()`**
쿼리 세트의 요소가 반환되는 순서를 바꿉니다. `reverse ()`를 다시 호출하면 순서가 정상 방향으로 복원됩니다.

```python
# queryset에서 "마지막"다섯 항목을 검색

my_queryset.reverse()[:5]
```
또한 `reverse ()`는 일반적으로 정의 된 순서가있는 QuerySet에서만 호출되어야합니다. 주어진 QuerySet에 대해 그러한 정렬이 정의되어 있지 않으면 `reverse ()`를 호출하면 실제 효과가 없습니다.

-
##### distinct()
**`distinct(*fields)`**

SQL 쿼리에서 SELECT DISTINCT를 사용하는 새 QuerySet을 반환합니다. 이렇게하면 조회 결과에서 **중복 행이 제거**됩니다.

기본적으로 QuerySet은 중복 행을 제거하지 않습니다. 실제로 `Blog.objects.all ()`과 같은 간단한 쿼리는 결과 행이 중복 될 가능성이 있기 때문에 거의 문제가되지 않습니다. 그러나 쿼리가 여러 테이블에 걸쳐있는 경우 QuerySet을 평가할 때 중복 결과를 얻을 수 있습니다. 그것은 `distinct ()`를 사용할 때입니다.

>`order_by ()` 호출에 사용 된 모든 필드는 SQL SELECT 열에 포함됩니다. `distinct ()`와 함께 사용하면 예기치 않은 결과가 발생할 수 있습니다. 관련 모델의 필드별로 주문하면 해당 필드가 선택한 열에 추가되고 그렇지 않으면 중복 행이 구별되는 것처럼 보일 수 있습니다. 추가열은 반환 된 결과에 표시되지 않으므로 (순서 지정을 지원할 때만 있음) 때로는 뚜렷하지 않은 결과가 반환되는 것처럼 보입니다.
>`alues ​​()` 쿼리를 사용하여 선택된 열을 제한하면 모든 `order_by ()` (또는 기본 모델 순서)에 사용 된 열이 여전히 관련되어 결과의 고유성에 영향을 줄 수 있습니다.
>`distinct ()`와 `values ​​()`를 함께 사용할 때 `values ​​()` 호출이 아닌 필드로 정렬 할 때는 주의해야합니다.

-
##### values()
**`values(*fields)`**

iterable(반복 가능한)로 사용될 경우 모델 인스턴스가 아닌 사전을 반환하는 QuerySet을 반환합니다. 각 사전은 모델 오브젝트의 속성 이름에 해당하는 키와 함께 오브젝트를 나타냅니다.

```python
# values ​​()의 사전을 일반 모델 객체와 비교합니다.
# This list contains a Blog object.
>>> Blog.objects.filter(name__startswith='Beatles')
<QuerySet [<Blog: Beatles Blog>]>

# This list contains a dictionary.
>>> Blog.objects.filter(name__startswith='Beatles').values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
```
`values ​​()` 메소드는 SELECT가 제한되어야하는 필드 이름을 지정하는 선택적 위치 인수, * 필드를 취합니다. 필드를 지정하면 각 사전에 지정한 필드의 필드 키 / 값만 포함됩니다. 필드를 지정하지 않으면 각 사전에는 데이터베이스 테이블의 모든 필드에 대한 키와 값이 포함됩니다.

```python
>>> Blog.objects.values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
>>> Blog.objects.values('id', 'name')
<QuerySet [{'id': 1, 'name': 'Beatles Blog'}]>
```
`ForeignKey` 인 `foo`라는 필드가있는 경우 실제 값을 저장하는 숨겨진 모델 속성의 이름이므로 `default values ​​()` 호출은 `foo_id`라는 사전 키를 반환합니다. 값 ()을 호출하고 필드 이름을 전달할 때 foo 또는 foo_id를 전달할 수 있으며 동일한 것을 다시 얻을 수 있습니다.

```python
>>> Entry.objects.values()
<QuerySet [{'blog_id': 1, 'headline': 'First Entry', ...}, ...]>

>>> Entry.objects.values('blog')
<QuerySet [{'blog': 1}, ...]>

>>> Entry.objects.values('blog_id')
<QuerySet [{'blog_id': 1}, ...]>
```
- `istinct ()`와 함께 `values ​​()`를 사용할 때 순서가 결과에 영향을 줄 수 있음에 유의하십시오. 
- `extra ()` 호출 후에 `values ​​()` 절을 사용하면 `extra ()`의 select 인수로 정의 된 모든 필드가 `values ​​()` 호출에 명시 적으로 포함되어야합니다. `values ​​()` 호출 후에 만들어진 `extra ()` 호출은 여분의 선택된 필드를 무시하게됩니다.
- `values ​​()` 뒤에 `only ()` 및 `defer ()`를 호출하면 의미가 없으므로 NotImplementedError가 발생합니다.

소수의 사용 가능한 필드에서 값만 필요하므로 모델 인스턴스 객체의 기능이 필요하지 않을 때 유용합니다. 사용할 필드 만 선택하는 것이 더 효율적입니다.
`values ​​()` 호출 뒤에 `filter ()`, `order_by ()` 등을 호출 할 수 있습니다. 이는 두 호출이 동일 함을 의미합니다.

```python
Blog.objects.values().order_by('id')
Blog.objects.order_by('id').values()
```

-
##### values_list()
**`values_list(*fields, flat=False)`**

사전을 반환하는 대신 반복되는 경우 튜플을 반환한다는 점을 제외하고는 `values ​​()`와 유사합니다. 각 튜플에는 `values_list ()` 호출로 전달 된 각 필드의 값이 들어 있으므로 첫 번째 항목이 첫 번째 항목이됩니다. 

```python
>>> Entry.objects.values_list('id', 'headline')
[(1, 'First entry'), ...]
```
단일 필드 만 전달하면 flat 매개 변수를 전달할 수도 있습니다. True이면 반환 된 결과가 단일 튜플이 아닌 단일 값임을 의미합니다.

```
>>> Entry.objects.values_list('id').order_by('id')
[(1,), (2,), (3,), ...]

>>> Entry.objects.values_list('id', flat=True).order_by('id')
[1, 2, 3, ...]
```
둘 이상의 필드가있을 때 플랫으로 전달하는 것은 오류입니다. `values_list ()`에 값을 전달하지 않으면 모델의 모든 필드가 선언 된 순서대로 반환됩니다.
일반적인 요구 사항은 특정 모델 인스턴스의 특정 필드 값을 가져 오는 것입니다. 이를 달성하려면 `values_list ()` 다음에 `get ()` 호출을 사용하십시오.

```python
>>> Entry.objects.values_list('headline', flat=True).get(pk=1)
'First entry'
```

`values ()`와 `values_list ()`는 모두 특정 유스 케이스의 최적화로 예정되어 있습니다.

```python
ManyToManyField를 통해 쿼리 할 때의 동작을 확인할 수 있습니다.

>>> Author.objects.values_list('name', 'entry__headline')
[('Noam Chomsky', 'Impressions of Gaza'),
 ('George Orwell', 'Why Socialists Do Not Believe in Fun'),
 ('George Orwell', 'In Defence of English Cooking'),
 ('Don Quixote', None)]
```
여러 항목이있는 작성자는 여러 번 나타나고 항목이없는 작성자는 항목 제목에 대해 없음을 갖습니다.
마찬가지로 역 외래 키를 쿼리 할 때 작성자가없는 항목에 대해서는 없음이 나타납니다.

```
>>> Entry.objects.values_list('authors')
[('Noam Chomsky',), ('George Orwell',), (None,)]
```

-
##### dates()
**dates(field, kind, order='ASC')**

`QuerySet`의 내용 내에서 특정 종류의 사용 가능한 모든 날짜를 나타내는 `datetime.date` 객체의 목록으로 평가되는 `QuerySe`t을 반환합니다.

##### datetimes()
##### none()
##### all()
##### select_related()
##### prefetch_related()
##### extra()
##### defer()
##### only()
##### using()
##### select_for_update()
##### raw()

#### Methods that do not return QuerySets
##### get()
##### create()
##### get_or_create()
##### update_or_create()
##### bulk_create()
##### count()
##### in_bulk()
##### iterator()
##### latest()
##### earliest()
##### first()
##### last()
##### aggregate()
##### exists()
##### update()
##### delete()
##### as_manager()

#### Field lookups
##### exact
##### iexact
##### contains
##### icontains
##### in
##### gt
##### gte
##### lt
##### lte
##### startswith
##### istartswith
##### endswith
##### iendswith
##### range
##### date
##### year
##### month
##### day
##### week_day
##### hour
##### minute
##### second
##### isnull
##### search
##### regex
##### iregex

#### Aggregation functions
##### expression
##### output_field
##### **extra
##### Avg
##### Count
##### Max
##### Min
##### StdDev
##### Sum
##### Variance

### Query-related tools
#### Q() objects
#### Prefetch() objects
#### prefetch_related_objects()
