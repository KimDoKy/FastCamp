# Python 미흡한 부분 추가 스터디

**두서 없이 개념이 흐릿한 것만 다시 모아서 study**

## 2017/1/31
#### join 

```python
>>> food = ["123","자장면","짬뽕","탕수육"]
   
>>> ','.join(food)
 '123,자장면,짬뽕,탕수육'

>>> '/'.join(food)
 '123/자장면/짬뽕/탕수육'
```
문자열(str)으로 반환

#### split

```python
>>> b = 'a,s,d,f,v,f.4.s.d.d'

>>> b.split('.')
 ['a,s,d,f,v,f', '4', 's', 'd', 'd']
>>> b.split(',')
 ['a', 's', 'd', 'f', 'v', 'f.4.s.d.d']
```
리스트(list)으로 반환

#### zip

```python
>>> list(zip([1,2,3,],[4,5,6]))
 [(1, 4), (2, 5), (3, 6)]

>>> list(zip([1,'d','r',4],[2,3]))
 [(1, 2), ('d', 3)]
```


#### enumerate

```python
>>> asd = ['asd','qw','zxc','vff','regg']

>>> for i in enumerate(asd):
       print(i)

 (0, 'asd')
 (1, 'qw')
 (2, 'zxc')
 (3, 'vff')
 (4, 'regg')
```

#### Literals
`12` , `3.14` 와 같은 숫자나
`'Les't study Python'`과 같은 문자열을 말함.
프로그램 내에서 직접 문자 형태로 지정되는 값들이기 떄문. **한번 지정되면 변하지 않음**.

#### Comprehension
##### 리스트 컴프리헨션
```
[표현식 for 항목 in iterable객체]
```
```python
>>> numbers = []
>>> for item in range(1, 6):
       numbers.append(item)
 
>>> numbers
[1, 2, 3, 4, 5]
```
개인 테스트

```python
>>> result = ['{} x {} = {}'.format(i,j,i*j) for i in range(2,10) for j in range(1,10)]
```
리스트 컴프리헨션을 사용할 경우

```python
>>> [item for item in range(1, 6)]
[1, 2, 3, 4, 5]
```
for문을 중첩으로 사용시 컴프리헨션

```python
for color in colors:
  for fruit in fruits:
  
[(color, fruit) for color in colors for fruit in fruits]
```
##### 셋 컴프리헨션
`{표현식 for 표현식 in iterable객체}`
##### 제네레이터 컴프리헨션
`(표현식 for 표현식 in interable객체)`

괄호로 되어있지만 튜플을 생성하지 않는다. (튜플은 컴프리헨션이 없다)  
제너레이터 객체는 순회 가능하며, 리스트 형태로 만들 수 있다.

## Sequence Types

### list
순차적인 데이터를 나타냄. 내부 항목을 변경 할 수 있다.

```python
saple_list = ['a','b','c','d']
```
다른 데이터를 list로 변환

```python
>>> list('I have a pen')
 ['I', ' ', 'h', 'a', 'v', 'e', ' ', 'a', ' ', 'p', 'e', 'n']
```
##### append(리스트 항목추가)

```python
>>> alpha = ['a', 'b', 'c', 'd']
>>> alpha_list.append('e')
>>> alpha_list
['a', 'b', 'c', 'd', 'e']
```
##### extend(리스트 병합)

```python
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.extend(colors)
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```
##### insert(특정위치 삽입)

```python
>>>numbers = [1,2,3,4,5]
>>> numbers.insert(2,8)
>>> numbers
 [1, 2, 8, 3, 4, 5]
```
##### del(특정 위치 삭제)

```python
>>> del numbers[4]
>>> numbers
 [1, 2, 8, 3, 5]
```
##### remove(값으로 삭제)

```python
>>> numbers.remove(3)
>>> numbers
 [1, 2, 8, 5]
```
##### pop(항목 추출 후 삭제)

```python
>>> numbers.pop(-1)
 5
>>> numbers
 [1, 2, 8]
```
##### index(값으로 오프셋 찾기)

```python
>>> numbers
 [1, 2, 8]
>>> numbers.index(8)
 2
```
##### in(존재여부 확인)

```python
>>> numbers
 [1, 2, 8]
>>> 8 in numbers
 True
```
##### count(값 세기)

```python
>>> numbers
 [1, 2, 8, 1, 1]
>>> numers.count(1)
 3
```
##### sort,sorted(정렬하기)
- sort

```python
>>> numbers
 [1, 2, 8, 1, 1]
>>> numbers.sort()
>>> numbers
 [1, 1, 1, 2, 8]
```
- sorted
> 입력 값을 정렬하고 결과를 리스트로 반환

```python
>>> sorted(['a', 'c', 'b'])
['a', 'b', 'c']
>>> sorted("zero")
['e', 'o', 'r', 'z']
```
##### copy

```python
>>> numbers
 [1, 1, 1, 2, 8]
>>> num = numbers.copy()
>>> num
 [1, 1, 1, 2, 8]
```

## 2017/2/

### tuple


set
dictionary
`{Key1:Value1, Key2:Value2, Key3:Value3 ...}`
lambda
decorator
generator
Regex
Crawling
Sequence Type
SQL
class,static method
attribute access modifier
get/set

### algorithm
