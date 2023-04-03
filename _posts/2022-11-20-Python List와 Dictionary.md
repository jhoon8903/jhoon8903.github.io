---
title: Python List와 Dictionary
category: python
author: "이정훈"
tags: [python]
img : https://blog.kakaocdn.net/dn/bL8ETY/btrsc1dKdvU/PKl3b1kLTKsKuWAY9u2XT1/img.png
comments_disable: true
meta_description: "Python List와 Dictionary"
---

# 리스트 (list)

### 리스트 기초

-   순서가 있는, 다른 자료형들의 모임

```python
a = [1,5,2]
b = [3,'a',6,1]
c = []
d = list()
e = [1,2,4,[2,3,4]]
```

-   리스트의 길이도 len() 함수를 이용 가능

```python
print(len(a))			# 3
print(len(b))			# 4
print(len(d))			# 0
print(len(e))			# 4
```

-   순서가 있기 떄문에, 문자열에서처럼 indexing 과 slicing 가능

```python
print(e[2])				# 4
print(e[3][1])			# 3
```

#### 리스트의 더 많은 기능들

-   덧붙이기

```python
a = [1,2,3]
a.append(5)
print(a)				# [1,2,3,5]

a.append([1,2])
print(a)				# [1,2,3,5,[1,2]]

a += [2,7]
print(a)				# [1,2,3,5,[1,2],2,7]
```

-   정렬하기

```python
a = [2,5,3]
a.sort()
print(a)				# [2,3,5]

a.sort(reverse=True)
print(a)				# [5,3,2]
```

-   요소가 list 안에 있는지 알아보기

```python
a = [2,1,4,"2",6]

print(1 in a)			# True
print('1' in a)			# False
print(0 not in a)		# True
```

***

# 딕셔너리 (dictionary)

### 딕셔너리 기초 (이하 dict)

-   dict는 'key'와 'value'의 쌍으로 이루어진 자료모임  
    ex. ) 영한사전에서 영어단어를 검색하면 한국어 뜻이 나오는 경우

```python
person = {"name":"bob", "age":21}
print(person["name"])				# 'bob'
```

-   dict 요소에는 순서가 없기 떄문에 indexing 불가 (Key Error)
-   dict 값을 업데이트 하거나 새로운 쌍의 자료를 넣을 수 있다.

```python
person = {"name":"bob", "age":21}

person['name'] = 'Robert'
print(person) 			# {'name': 'Robert', 'age': 21}

person['height'] = 174.8
print(person)			# {'name': 'Robert', 'age': 21, 'height': 174.8}
```

-   dict의 Value로는 아무 자료형이나 쓸 수 있어, 다른 dict를 넣을 수 있다.

```python
person = {'name':'Alice',
          'age':16,
          'scores':{'math':81, 'science':92, 'Korean':84}}
print(person['scores'])

> {'math': 81, 'science': 92, 'Korean': 84}

print(person['scores']['science'])

> 92
```

-   dcit 안에 해당 key가 존재하는지 알고 싶을 때는 in을 사용

```python
person = {'name' : 'Bob', 'age' : 21}

print('name' in person) 		# True
print('email' in person)		# Fales
print('phone' not in person)	# True
```

### list와 dict의 조합

-   dict는 list와 함께 쓰여 자료를 정리하는데 쓰일 수 있다.

```python
people = [{'name':'bob','age':20,},{'name':'carry','age':38}]

print(people[0]['name']) 		# 'bob'
print(people[1]['name'])		# 'carry'

person = {'name':'john', 'age':7}
people.append(person)
print(people)
> [{'name': 'bob', 'age': 20}, 
   {'name': 'carry', 'age': 38}, 
   {'name': 'john', 'age': 7}]
```