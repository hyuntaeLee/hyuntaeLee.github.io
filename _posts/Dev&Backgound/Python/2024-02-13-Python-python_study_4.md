---
layout: post
title: 【PY_04】Python Study
subtitle: with book
categories: Anything
banner:
  image: https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/8a91cce1-f6ff-4080-a59b-625b14f1737f
tags: [python, study]
---

## 01.유요한 펠린드롬

**펠린드롬 이란?**

**앞뒤가 똑같으 단어나 문장으로, 뒤집어도 같은 말이 되는 단어 또는 문장을 펜린드롬이라고 한다.** 

### 1-1리스트로 변환

```python
#전처리 과정 
strs = []
for char in s:
	if char.isalnum(): #isalnum()를 사용해서 판별
		strs.append(char.lower())
```

직접 문자열을 받아서 처리하는데 그러기 위해서 영문자, 숫자를 판별하는 함수인 `**isalnum()`** 를 사용해서 문자만을 추가한다. 대소문자를 구분하지 않기 때문에 `lower()` 를 사용해서 소문자로 변환한다

```python
#펜린드롬 여부 파악
while len(str) > 1:
	if strs.pop(0) != strs.pop()
		return False
```

pop()은 원래 해당 문자열에서 마지막 문자를 pop시키는데, pop()는 인덱스를 설정할수 있는걸 이용해서 0번째에 있는 문자를 pop시키면서 비교한다. 그래서 일치하지않는 경우 False를 리턴한다.

```python
def isPalindrome(self, s: str) -> bool:
	strs = []
	for char in s:
		if char.isalnum():
			strs.append(char.lower())

	while len(strs) > 1:
		if strs.pop(0) != strs.pop():
			return Flase

return True
```

### 1-2. 데크 자료형을 이용

```python
def isPalindrome(self, s: str) -> bool:
        strs: Deque = collections.deque() #데크 자료형 사용

        for char in s:
            if char.isalnum():
                strs.append(char.lower())

        while len(strs) > 1:
            if strs.popleft() != strs.pop():
                return False
        
        return True
```

> 데크 자료형을 사용한거만으로 #1리스트 풀이에 비해 5배 가까이 더 속도를 높일 수 있다.
> 
> 
> <aside>
> 💡 **`pop(0)`** in a list: **`pop(0)`**은 리스트에서 첫 번째 요소를 제거하는 연산입니다. 하지만 리스트에서 첫 번째 요소를 제거하면 나머지 요소들을 모두 앞으로 이동시켜야 합니다. 따라서 **`pop(0)`**은 리스트의 길이에 비례하는 시간이 소요됩니다. 따라서 리스트의 길이가 n일 때, **`pop(0)`**의 시간 복잡도는 **O(n)**입니다.
> 
> **`popleft()`** in a deque: **`popleft()`**는 데크에서 첫 번째 요소를 제거하는 연산입니다. 데크는 양쪽 끝에서의 삽입과 제거가 효율적으로 이루어지기 때문에 **`popleft()`**는 상수 시간에 수행됩니다. 따라서 **`popleft()`**의 시간 복잡도는 **O(1)**입니다.
> 
> </aside>
> 
> 해당 이유 때문에 성능차이가 많이 난다.
> 

### 1-3. 슬라이싱 사용

```python
def isPalidrome(self, s: str) -> bool:
	s = s.lower()
	s = re.sub('[^a-z0-9]', '', s)

	return s == s[::-1] #슬라이싱
```

앞에 설명들은 isalnum()를 사용해서 모든 문자를 일일이 검사했지만, 
여기선 `re.sub('[^a-z0-9]', '', s)` 를 사용해서 불필요한 문자들을 두번째 매개변수인 빈칸으로 그니까 없앤다.

그리고 마지막 s[::-1]를 이용해서 문자열을 뒤집어서 비교한다.
> 

| 알고리즘 | 실행 시간 | 슬라이싱을 1로 했을때 |
| --- | --- | --- |
| 슬라이싱 | 0.499마이크로초 | 1 |
| 리스트 reverse() | 2.46 | 5 |
| reverse() + join() | 2.49 | 6 |
| for | 5.5 | 12 |
| while | 9.4 | 21 |
| 재귀 | 24.3 | 54 |

표를 확인했듯이 **슬라이싱**은 파이썬이 제공하는 기능 중 속도가 가장 빠르면서도 매우 편리하다

---

## 02.문자열 뒤집기

문자열을 뒤집는 함수를 작성해라. 입력값은 문자열 배열이며, **리턴 없이 리스트 내부를 직접 조작해라**

### 2-1.투 포인터를 이용한 스왑

```python
def reverseStr(self, s: str) -> None:
	left, right = 0, len(s) - 1
	while left < right:
			s[left], s[right] = s[right], s[left] #해당 부분에서 왼쪽 오른쪽 교체됨
			left += 1
			right -= 1
```

 투 포인터(Two Pointer)를 이용한 전통적인 방식으로 풀어본다. 
**”리턴 없이 리스트 내부를 직접 조작하라” 라는 제약 사항**이 있으므로 다음과 같이 s내부를 스왑하는 형태로 풀이하면 된다


### 2-2.파이썬다운 방식

```python
#reverse()함수 사용
def reverseStr(self, s: str) -> None:
	s.reverse()

#슬라이싱1 => 리트코드에서 오류
s = s[::-1]

#슬라이싱2 => 슬라이싱1 개선
s[:] = s[::-1]
```

> reverse()함수를 사용해서 풀면 처음 함수보다 빠르며 가장 파이썬 다운 방식이다.

#슬라이싱 1
은 슬라이싱을 통해서 문자열이 되긴 되는데, 리트코드에서는 정상적으로 처리가 되지 않는다.
그래서 #슬라이싱2를 통해서 슬라이싱1을 개선할 수 있다.
> 

```python
def reverseString(self, s: List[str]) -> None:
        s[:] = s[::-1]
        return s
```

> 그래서 #슬라이싱2를 통해서 해당 함수를 만들 수 있다.
> 

---

### 03.로그파일 재정렬

**로그를 다음 기준으로 재정렬하라.**

- **로그의 가장 앞 부분은 식별자다.**
- **문자로 구성된 로그가 숫자 로그보다 앞에 온다.**
- **식별자는 순서에 영향을 끼치지 않지만, 문자가 동일한 경우 식별자 순으로 한다.**
- **숫자 로그는 입력 순서대로 한다.**

```python
if log.split()[1].isdigit():
	digits.append(log)
else:
	letter.append(log)
```

기준을 인지하고 문자로 구성된 로그가 숫자로 구성된 로그보다 앞에 있어야한다.

1. log.split()을 통해서 문자열을 공백 기준으로분리한다
2. 문자열 의 [1]인덱스 부터 숫자가 있기 때문에 [1]에서 isdigit()함수로 구분
  -**isdigit()는 숫자인지 아닌지 구분한다.**
> 

```python
letters.sort(key=lamda x: (x.split()[1:], x.split()[0]))
```



## reference

해당 포스트은 "파이썬 알고리즘 인터뷰" 책을 구매해서 내용을 정리한 포스트입니다.  
[책 구매하기](https://www.yes24.com/Product/Goods/91084402)
<p align="center">
<img src="https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/bf17718d-042e-40d1-acb4-72b0e7ba1773" width = 300>
</p>