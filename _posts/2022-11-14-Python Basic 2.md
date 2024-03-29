---
layout : post
title: Python Basic 2
subtitle : Python define, loop, conditional에 대해서
author: Daniel
categories: Python
tags:
 - Python
 - Programming
 - Language
banner:
 image : https://blog.kakaocdn.net/dn/bL8ETY/btrsc1dKdvU/PKl3b1kLTKsKuWAY9u2XT1/img.png
---

함수 (def)
--
## 함수의 정의

### 이름은 선택에 따라 결정가능

```null
수학문제에서 
f(x) = 2*x+3 / y = f(2) 일때 y의 값은? '7'

Javascript 
function f(x) {
	return 2*x+3
  }
  
Python
def f(x) : 
	return 2*x+3
    
 y = f(2) = 7
```

-   함수의 응용

```null
def sum_all(a,b,c) :
	return a+b+c

def mul(a,b) :
	return a*b

result = sum_all(1,2,3) + mul(10,10)

print(result)							# result 값 출력

106
```

## 조건문

-   if / else 로 구성

```null
def oddeven(num) : 		# oddeven이라는 이름의 함수를 정의한다. (num)을 변수로 받는다.
  if num % 2 == 0 :		# num을 2로 나눈 나머지가 0이면,
  	return True			# True(참)을 반환한다.
  else :				# 아니면
  	return False		# False(거짓)을 반환한다.
    
result = oddeven(20)

print(result)

True
```

```null
def is_adult(age) : 
	if age >= 20 :
       print('성인입니다.')    # 조건(age)이 true(참:20보다 크거나 같은)면, '성인입니다.' 출력
    else :
       print('청소년입니다.')  # 조건(age)이 False(거짓:20보다 작은)면, '청소년입니다.' 출력
       
is_adult(30) = '성인입니다.'
is_adult(20) = '성인입니다.'
is_adult(19) = '청소년입니다.'
```

## 반복문

### Python의 반복문은, list의 요소들을 하나씩 꺼내쓰는 형태

```null
fruits = ['사과','배','감','귤']

for fruit in fruits:
	print(fruit)
    
    > 사과
      배
      감
      귤
```

```null
fruits = ['사과','배','배','감','수박','귤','딸기','사과','배','수박']
count = 0
for fruit in fruits:
    if fruit == '사과':
        count += 1
print(count) 			# 사과의 갯수를 세어 보여줍니다. 

> 2
```

```null
fruits = ['사과','배','배','감','수박','귤','딸기','사과','배','수박']

def count_fruits(target):
    count = 0
    for fruit in fruits:
        if fruit == target:
            count += 1
    return count

subak_count = count_fruits('수박')
print(subak_count)					# 수박의 갯수 출력

gam_count = count_fruits('감')
print(gam_count)					# 감의 갯수 출력

sagwa_count = count_fruits('사과')
print(sagwa_count)					# 사과의 갯수 출력

bae_count = count_fruits('배')
print(bae_count)					# 배의 갯수 출력

> 2
  1
  2
  3
```

### Dictionry 예제

```null
people = [{'name':'bob','age':20},
		  {'name':'carry','age':38},
          {'name':'john','age':7},
          {'name':'smith','age':17},
          {'name':'ben','age':27}]        

for person in people:
	print(person['name'],person['age']) 	
    
## people 안에 있는 person의 ['name','age'] 값 출력 ##
    
> bob 20
  carry 38
  john 7
  smith 17
  ben 27
```

```null
people = [{'name': 'bob', 'age': 20},
          {'name': 'carry', 'age': 38},
          {'name': 'john', 'age': 7},
          {'name': 'smith', 'age': 17},
          {'name': 'ben', 'age': 27}]


def get_age(truename):		# 'name'을 받으면, 'age'를 return해주는 함수
    for person in people:
        if person['name'] == truename:
            return person['age']
    return '해당하는 이름이 없습니다.'		


print(get_age('bob'))
> 20

print(get_age('kay'))
> 해당하는 이름이 없습니다.
```