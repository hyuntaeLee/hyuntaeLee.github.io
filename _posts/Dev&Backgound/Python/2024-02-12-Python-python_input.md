---
layout: post
title: 【PY_etc】 Python Input
subtitle: sys.stdin.readline
categories: Python
banner:
  image: https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/8a91cce1-f6ff-4080-a59b-625b14f1737f
tags: [python, study]
---

## sys.stdin.readline을 사용하려는 이유

코테 준비할겸, Python 공부할겸 심심할때 문제를 풀고 있습니다.  

풀때마다 입력을 받을때 `input()`을 사용해서 값을 입력을 받는데, 1~2줄 받는건 괜찮은데 2~3줄 넘어가면 이상하게 런타임 에러가 발생했다.

그래서 [링크](https://www.youtube.com/watch?v=QqB3OHMpeaY&list=PLzw48D--UxsHsOQ1PTEoXPJSYfe3StjOG&index=3)에 있는 내용을 보고 간단하게 정리하려고 한다.

```
입력
5
10 5
20 6
30 7
50 123
```

```python
import sys

T = int(input()) #Test case
for i in range(T):
        a,b = map(int, sys.stdin.readline().split())
        print(a+b)
```

입력이 위와 같을때, 맨 첫줄 Test case를 입력받을 때는 `input()`을 사용해도 무방하다.
하지만 반복문으로 여러줄 입력받는 상황에서는 반드시 `sys.stdin.readline()`을 사용해야 시간초과가 발생하지 않는다고 설명한다.

### 한 개의 정수를 입력받을때

```python
import sys
a = int(sys.stdin.readline())
```

하지만, `sys.stdin.readline()`은 한줄 단위로 입력받기에, 개행문자도 같이 입력받게 된다.  
그리고 해당 값이 문자열 형태로 저장되기 때문에 `int()`를 사용해서 형변환을 해줘야한다.

### 정해진 개수의 정수를 한줄에 받을때

```
입력
1 2 3
```

```python
import sys
a,b,c = map(int,sys.stdin.readline().split())
```

`map()`은 반복 가능한 객체(리스트 등)에 대해 각각의 요소들을 지정된 함수로 처리해주는 함수이다.  
그래서 위에 코드는 `int()`를 명시해서 형변환을 해준다.

### 임의의 개수의 정수를 한줄에 입력받아 리스트에 저장할 때

```
입력
4 5 6
```

```python
import sys
data = list(map(int,sys.stdin.readline().split()))
```

`split()`을 사용해서 공백(스페이스, 탭, 엔터 등)을 기준으로 문자열을 나눠준다.

### 임의의 개수의 정수를 n줄 입력받아 2차원 리스트에 저장할 때

```python
import sys
data = []
n = int(sys.stdin.readline())
for i in range(n):
    data.append(list(map(int,sys.stdin.readline().split())))
```

이렇게 구현하면 각 요소의 길이가 동일한 2차원 리스트도 만들 수 있고, 각 각 길이가 다른 2차원 리스트도 입력 받을 수 있다.

### 문자열 n줄을 입력받아 리스트에 저장할 때

```python
import sys
n = int(sys.stdin.readline())
data = [sys.stdin.readline().strip() for i in range(n)]
```

`strip()`을 사용해서 문자열의 맨앞, 맨뒤의 공백문자를 제거한다.

## reference

[https://velog.io/@yeseolee/Python-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9E%85%EB%A0%A5-%EC%A0%95%EB%A6%ACsys.stdin.readline](https://velog.io/@yeseolee/Python-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9E%85%EB%A0%A5-%EC%A0%95%EB%A6%ACsys.stdin.readline)