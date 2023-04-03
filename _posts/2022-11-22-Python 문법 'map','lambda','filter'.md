---
title: Python 문법 'map','lambda','filter'
category: python
author: "이정훈"
tags: [python]
img : Python 문법 'map','lambda','filter'
comments_disable: true
meta_description: "Python 문법 'map','lambda','filter'"
---

# map

-   data_list

```python
people = [{'name': 'bob', 'age': 20},
          {'name': 'carry', 'age': 38},
          {'name': 'john', 'age': 7},
          {'name': 'smith', 'age': 17},
          {'name': 'ben', 'age': 27},
          {'name': 'bobby', 'age': 57},
          {'name': 'red', 'age': 32},
          {'name': 'queen', 'age': 25},
          ]
```

-   map을 이용하여 리스트 결과값 출력 (1차)

```python
def check_adult(person):
    if person['age'] >= 20:
        return '성인'
    else:
        return '청소년'
result = map(check_adult, people)
print(list(result))

> ['청소년', '성인', '청소년', '청소년', '성인', '성인', '성인', '성인']
```

-   map, if-삼항연산자 이용하여 리스트 결과값 출력 (2차)

```python
def check_adult(person):
   	return '성인' if person['age'] >= 20 else '청소년'
result = map(check_adult, people)
print(list(result))
```

-   map, if-삼항연산자, lambda를 이용하여 리스트 결과값 출력 (3차)

```python
result = map(lambda x: ('성인' if x['age'] >= 20 else '청소년'), people)
print(list(result))
```

# filter

-   map과 유사하며, True 값만 취하기

```python
result = filter(lambda x: x['age'] >= 20, people)
print(list(result))

> 
[{'name': 'bob', 'age': 20}, 
{'name': 'carry', 'age': 38}, 
{'name': 'ben', 'age': 27}, 
{'name': 'bobby', 'age': 57}, 
{'name': 'red', 'age': 32}, 
{'name': 'queen', 'age': 25}
]
```