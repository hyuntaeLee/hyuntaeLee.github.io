---
layout: post
title: 【PY_01】Python Study
subtitle: with book
categories: Python
banner:
  image: https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/8a91cce1-f6ff-4080-a59b-625b14f1737f
tags: [python, study]
---

## Basic of Python-1

### Loop

```cpp
//C++
int sum = 0;
for(int i = 0; i <= 10; i++) {
	sum += i;
}
```

```python
#Python

#1
sum = 0
for i in range(1, 10+1):
	sum += i;

#2
sum = sum(i for i in range(1, 10+1))

#3
sum = sum(range(1, 10 + 1))
```

> C++과 다르게 Python은 한줄로도 표현이 가능하지만, 지나치게 사용하면 가독성을 떨어뜨림 적절하게 사용해야한다.
> 
> 
> <aside>
> 💡 `sum(iterable, start=0)`
> sum()함수는 반복 가능한(iterable)객체(리스트, 튜플, 집합 등)의 원소들을 모두 더한 갑을 반환한다
> 
> </aside>
> 
> `sum()` 함수를 사용해서 `range()` 함수로 지정된 범위 내의 정수를 반환해서 반복가능한 객체를 생성해서, 합을 구한다.
> 

---

### **Generic programing(제너릭 프로그래밍)**

> 제너릭(generic)이란 파라미터의 타입이 나중에 지정되게 해서 재활용성을 높일 수 있는 프로그래밍 스타일이다.
> 

```cpp
//C++
template<class T, class U>
bool are_equal(T a, U b) {
	return (a == b);
}
```

```python
#1 
def are_equal(a, b):
	return a == b

are_equal(10, 10.0)

#2
from typing import TypeVar

def are_eauql(a: T, b: U) -> bool:
	return a == b

are_equal(10, 10.0)
```

> 파이썬은 원래 동적 타이핑 언어이기 때문에 **제너릭이 필요 없다**.

**하지만 동적 타이핑의 장점이자 단점은 얼핏 사용하기엔 매우 편하지만 코드의 복잡도가 높아질 수록 혼란을 가중시킨다는 점이다**.

타입을 아예 명시하지 안흥면 가독성을 낮추고 버그 발생 확률이 높아진다. 따라서 **#2코드** 처럼 **타입을 명시할 수 있다.**
> 

---

### **배열 반복**

```cpp
//C++
std::string foo[] = {"A", "B", "C"};

for(std::string f : foo) {
	std::cout << f << std::endl;
} 
```

```python
#1
foo = ['A', 'B', 'C']

for i in foo:
	print(i)
```

> 파이썬은 역시 자료형 선언이 필요가 없는게 보이며, 이렇게 생략되서 상당히 간결하게 보인다.

하지만 이렇게 지나치게 간결하면 나중에 코드의 복잡도가 높아졌을때 가독성을 떨어뜨리는 요인이 된다.
> 

---

### 구조체

```cpp
//C++
struct Product {
	int weight;
	double price;
};

Product apple;
apple.price = 10;
```

```python
#Python 3.7+
from dataclasses import dataclass

@dataclass
class Product:
	weight: int = None
	price: float = None

apple = Product()
apple.price = 10 
```

> **파이썬에는 구조체가 없을뿐더러 클래스 또한 데이터 타입을 지정할 수 없다.**

하지만 필요한 경우 @dataclass 데코레이션을 사용할 수 있다. 이렇게 선언할 경우 초기화 함수를 지원하고, 출력할 때도 읽기 위운 포맷으로 출력해주는 등 여러 가지 편리한 기능을 제공한다.
> 

---

### Class

```cpp
//C++
class Rectangle {
	int width, height;
public:
	Rectangle(int, int);
	
	int area();
}

Rectangle::Rectangle(int x, int y) {
	width = x;
	height = y;
}

int Rectangle::area() {
	return width * height;
}

Rectangle rect(3, 4);
std::cout << rect.area() << std::endl;
```

```python
from dataclass import dataclass

@dataclass
class Rectangle
	width: int
	height: int

	def area(self):
		return self.width * self.height

rect = Rectangle(3, 4)
print(rect.area())
```

> 구조체 만들때 사용한 dataclass 데코레이션을 이용해 클래스를 선언했다. 물론 dataclass를 선언하지 않아도 클래스 구현에는 문제가 없다.
>

## reference

해당 포스트은 "파이썬 알고리즘 인터뷰" 책을 구매해서 내용을 정리한 포스트입니다.  
[책 구매하기](https://www.yes24.com/Product/Goods/91084402)
<p align="center">
<img src="https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/bf17718d-042e-40d1-acb4-72b0e7ba1773" width = 300>
</p>