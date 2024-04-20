---
layout: post
title: 【PY_00】Python Study
subtitle: with book
categories: Anything
banner:
  image: https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/8a91cce1-f6ff-4080-a59b-625b14f1737f
tags: [python, study]
---
# [Python] 기본-3

### 변수명과 주석

```python
def numMatchSubseq(self, S: str, words: List[str]) -> int:
		a = 0
	
		for b in words:
				c = 0
				for i in range(len(b)):
						d = S[c:].find(b[i])
						if d < 0:
								a -= 1
								break
						else:
								c += d +1
				a += 1
		
		return a
```

> 해당 코드는 좋은 코드로 보인다. 왜냐하면 파이썬은 인덴트(indentation)를 강제하므로, 얼핏 보기에 최소한 지저분해 보이지는 않는다.

하지만 해당 코드는 변수명이 무엇을 의미하는지를 이해하기 어려우며, 알고리즘에 대한 주석이 없어서 어떻게 동작하는지 파악하기 어렵다.
> 

```python
def numMatchingSubseq(self, S: str, words: List[str]) -> int:
		matched_count = 0

		for word in words:
				pos = 0
				for i in range(len(word)):
						#Find matching position for each character
						if found_pos < 0:
								matched_count -= 1
								break
						else: #If found, take step position forward
								pos += founds_ps + 1
				matched_count += 1

		return matched_count
```

> 지금 코드처럼 간단한게 주석을 부여하고, 의미 없는 변수명보다 각각의 의미를 부여해 작명했더니 위에 있던 코드보다 가독성이 좋아보인다.
> 

---

### 리스트 컴프리헨션

> 리스트 컴프리헨션은 파이썬의 매우 강력한 기능 중 하나이지만, 특유의 문법과 의미를 축약하여 나타내는 특징 탓에 지나치게 남발하게되면 파이썬의 가독성을 떨어트린다.
> 

```python
str1s = [str1[i:i + 2].lower() for i in range(len(str1) - 1) if re.findall('[a-z]{2}', str1[i:i + 2].lower())]
#str1문자열에서 2개씩 연속된 문자를 추출해 소문자로 만들고 리스트에 저장
```

> 이렇게 한줄로 표현도 좋지만 가독성이 좀 떨어지는걸 확인할 수 있다.
> 

```python
str1s = [
		str1[i:i + 2].lower() for i in range(len(str1) - 1) 
		if re.findall('[a-z]{2}', str1[i:i + 2].lower())
]
```

> 한줄로 적지말고 이렇게 라인을 좀 더 여유롭게 활용해서 가독성을 좀 높일수 있다.
> 

```python
str1s = []
for i in range(len(str1) - 1):
		if re.findall('[a-z]{2}', str1[i:i + 2].lower()):
				str1s.append(str1[i:i + 2].lower())
```

> 아니면 이렇게 가독성을 위해서 풀어 쓰는게 나쁘지 않을 수 있다.
>