---
layout: post
title: 【PY_02】Python Study
subtitle: with book
categories: Python
banner:
  image: https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/8a91cce1-f6ff-4080-a59b-625b14f1737f
tags: [python, study]
---

## Basic of Python-2

### 네이밍 커벤션

```python
#카멜 케이스
camelCase: int = 1

# 스네이크 케이스
snake_case: int = 1 
```

> 파이썬의 변수명 네이밍 컨벤션(naming Convention)은 자바와 달리 각 단어를 **밑줄”_” 로 구분**하여 표기하는 **스네이크 케이스(snake case)** 를 따른다. 이는 함수명도 마찬가지다. 

최소한 직접 작성한는 코드는 소문자 변수명과 함수명을 기본으로 해야한다.
> 

---

### 타입 힌트

```python
a: str = "1"
b: int = 1
```

> 파이썬은 대표적인 동적 타이핑 언어인데, 타입을 지정할 수 있는 타입 힌트(Type Hint)가 존재한다.
> 

```python
# 타입 힌트를 사용하지 않은 함수
def fn(a):
	...

# 타입 힌트를 사용하는 함수
def fn(a: int) -> bool:
```

> **타입 힌트를 사용하지 않는 함수**를 사용하면 빠르게 정의해서 사용할 수 있지만 `fn()` 함수의 파라미터 a에는 숫자를 넘겨야 하는지, 문자를 넘겨야 하는지 전혀 알 수 없다.
**⇒ 그래서 프로젝트 규모가 커지면 가독성이 떨어져 버그 유발**

하지만 **타입 힌트를 사용해서 만든 함수**는 파라미터 a가 정수형을 분명하게 알 수 있으며 리턴 값으로 True또는 False를 리턴할 것이라는 점도 확실하게 알 수 있다.
⇒**이렇게 명시함으로서 가독성도 좋아지고 버그유발도 줄인다.**
> 

---

### **List Comprehension(**리스트 컴프리헨션)

```python
#람다 표현식
>>> list(map(lambda x: x + 10, [1, 2, 3,]))
[11, 12 ,13]

#리스트 컴프리헨션(List Comprehension)
>>> [n * 2 for i in range(1, 10 + 1) if n % 2 == 1]
[2, 6, 10, 14, 18]

#리스트 컴프리헨션을 사용하지 않은 경우
>>> a = []
>>> for i in range(1, 10 + 1):
				if n % 2 == 1:
						a.append(n * 2)

#딕셔너리 컴프리헨션
a = { key: value for key, value in original.items()}

#딕셔너리 컴프리헨션 사용하지 않은 경우
>>> a = {}
>>> for key, value in original.items():
				a[key] = value

```

> 람다를 사용해서 첫번째 코드처럼 리스트를 만들수 있다.

하지만 사실 파이썬의 훨씬 더 유용한 기능은 **리스트 컴프리헨션(List Comprehension)**이다. 리스트 컴프리헨션이란 기존 리스트를 기반으로 새로운 리스트를 만들어내는 구문으로, 람다 표현식에 map이나 filter를 섞어서 사용하는 것에 비해 가독성이 훨씬 높다.

리스트 컴프리헨션을 사용하지 않은 코드를 확인해보면 사용한거에 비해 훨씬 더 길고 리스트 a 또한 따로 선언해줘야한다.
딕셔너리 또한 컴프리헨셔을 사용해서 표현할 수 있다. 

---

### Generator(제너레이터)

> 루프의 반복 동작을 제어할 수 있는 루틴 형태를 말한다.

예를 들어 임의의 조건으로 숫자 1억 개를 만들어낸 계산하는 프로그램을 작성한다고 가정해보자. 이 경우 제너레이터가 없다면 메모리 어딘가에 만들어낸 숫자 1억개를 보관하고 있어야 한다. 그러나 제너레이터를 이용하면, 단순히 제너레이터만 생성해두고 필요할 때 언제든 숫자를 만들어낼 수 있다. 

그리고 이때 `yield` 구문을 사용하면 제너레이터를 리턴할 수 있다.
> 

```python
>>> def get_num():
...     n = 0
...     while True:
...          n += 1
...          yield n
```

> 기존의 함수는 `return` 구문을 맞닥뜨리다면 값을 리턴하고 함수를 종료한다.

하지만 `yield` 는 제너레이터가 여기까지 실행 중이던 값을 내보낸다는 의미로, 중간값을 리턴한 다음 함수는 종료되지 않고 계속해서 맨 끝에 도달할 때까지 실행된다.
> 

```python
>>> get_num()
<generator object get_num at 0x0000028BEEA45000>

>>> g = get_num()
>>> for _ in range(0, 50):
...       print(next(g)) #next()함수로 다음값 생성
...
1
2
3
...
49
50
```

> 다음 값을 생성하기 위해서 next()로 추출한다.
> 

---

### range

> 제너레이터의 방식을 활용하는 대표적인 함수로 range()가 있다.
> 

```python
>>> list(range(5))
[0, 1, 2, 3, 4]

>>> range(5)
range(0, 5)

>>> type(range(5))
<class 'range'>

>>> for i in range(5):
...       print(i, end=' ')
...
0 1 2 3 4
```

```python
>>> a = [n for i in range(1000)]   #list 컴프리헨션
>>> b = range(1000)                #range 방식

>>> len(a)
1000
>>> len(b)
1000

>>> len(a) == len(b)  #len으로 비교해보면 둘 다 동일한 100개로 True 반환
True

>>> b
range(0, 1000)
>>> type(b)
<class 'range'>
```

```python
>>> sys.getsizeof(a)
8697464
>>> sys.getsizeof(b)
48
```

> 이제 a,b의 메모리 점유율을 비교해 보면 `range`클래스를 리턴하는 방식이 메모리 점유율이 **휠씬 더 적다.**

왜냐하면 리스트 a는 1000개의 요소가 포함된 리스트이며, 각 요소는 정수형 객체이므로 메모리 사용량이 크다.
**하지만 b는 단순히 0부터 999까지의 숫자를 가진 range객체이므로 매우 적은 메모리를 사용한다**


---

### enumerate

> `enumerate()` 는 “열거하다”는 뜻의 함수로, 여러 가지 자료형(list, set, tuple등)을 인덱스를 포함한 enumerate 객체로 리턴한다. 사용 방법은 다음과 같다.
> 

```python
>>> a = [1,2,3,2,45,2,2,5]
>>> a
[1,2,3,2,45,2,2,5]
>>> enumerate(a)
<enumerate object at 0x1010f83d0>

>>> list(enumerate(a))
[(0,1), (1,2), (2,3), (3,2), (4,45), (5,2), (6,5)]
```

> 그럼 liste()로 결과를 추출할 수 있는데, 인데스를 자동으로 부여해주기 때문에 매우 편리하게 활요할 수 있다. 

```python
#입력값
a = ['a1', 'b2', 'c3']

#1   for-range구문
for i in range(len(a)):
		print(i, a[i])

#2   for 구문
i = 0
for v in a:
	print(i, v)
	i += 1

#3   enumerate를 사용
for i, v in enumerate(a):
		print(i, v)

```

> `enumerate` 를 사용하면 더욱 깔끔하게 인덱스와 값을 한번 출력하는 코드를 만들 수 있다.
> 

---

### “//” 나눗셈 연산자

```python
>>> 5 / 3
1.6666666...7

>>> type(5 / 3)
<class 'float'>

# "//" 나눗셈 연산자는 int(a / b)와 동일하다
>>> 5 // 3
1

>>> type(5 // 3)
<class 'int'>

>>> int(5 / 3)
1

>>>type(int(5 / 3))
<class 'int'>

#나머지
>>> 5 % 3
2

#몫과 나머지를 한 번에 구할 때
>>> divmod(5, 3)
(1, 2)
```

---

### print

```python
>>> print('A1', 'B2')
A1 B2

#sep를 사용해서 구분자 지정
>>> print('A1', 'B2', sep=',')
A1,B2

#end를 사용해서 공백으로 처리해 줄바꿈을 하지않도록 할수있다.
print('aa', end=' ')
print('bb')

aa bb

#리스트를 출력할 때 join()으로 묶어서 처리한다
>>> a = ['A', 'B']
>>> print(' '.join(a))
A B

#format을 사용해서 출력
idx = 1
fruit = "Apple"

>>> print('{0}: {1}'.format(idx + 1, fruit))
2: Apple

#인덱스를 생략한 경우
>>> print('{}: {}'.format(idx + 1, fruit))
2: Apple

#더욱 간결한 표현
>>> print(f'{idx + 1}: {fruit}')
2: Apple
```

---

### Pass

```python
class Myclass(obj):
	def method_a(self):

	def method_b(self):
			print("ABC")

c = Myclass()
```

> 하지만 해당 코드는 실행이 되지 않는다. 왜냐하면 해당 코드에서 `method_a()` 가 아무런 처리를 하지 않았기 때문에 `method_b()` 에서 오류가 발생한다.

```python
class Myclass(obj):
	def method_a(self):
			#여기에 pass를 넣음
			pass

	def method_b(self):
			print("ABC")

c = Myclass()
```

> pass는 널 연산으로 아무것도 하지 않는 기능이다. 이렇게 아무역할을 하지 않는 pass를 지정하면, 앞서 발생한 인덴트 오류 같은 불필요한 오류를 방지한다.

## reference

해당 포스트은 "파이썬 알고리즘 인터뷰" 책을 구매해서 내용을 정리한 포스트입니다.  
[책 구매하기](https://www.yes24.com/Product/Goods/91084402)
<p align="center">
<img src="https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/bf17718d-042e-40d1-acb4-72b0e7ba1773" width = 300>
</p>