---
layout: post
title: MySQL JOIN
subtitle: MySQL Query JOIN
categories: MySQL
author: Daniel
tags: 
 - MySQL
 - Database
 - Query
banner:
 image : assets/images/posts/mysql.png
---

JOIN ( 여러 TABLE을 연결 )
--

 - 두 Table의 공통된 정보 (key value)를 기준으로 [한 Table]처럼 보는 것
	 - ex) user_id Field 기준으로 user 와 orders Table을 연결해서 한눈에 보는 것
			![](https://i.imgur.com/1AXutFl.png)

## 종류
	LEFT JOIN
![300](https://i.imgur.com/1grDrJl.png)

- SELECT * FROM users u 
  LEFT JOIN point_users p 
  ON u.user_id = p.user_id
![](https://i.imgur.com/hVUnUg7.png)

	LNNER JOIN
![300](https://i.imgur.com/LQopRGT.png)

- SELECT * FROM users u
  INNER JOIN point_users p
  ON u.user_id = p.user_id
![](https://i.imgur.com/fXlXkB2.png)


### 예제 1.
### orders Table 과 users Table 연결
#### LEFT JOIN ( 두 TABLE 은 users_id라는 같은 FieldName 이 있음)
   orders > users
   - SELECT * FROM orders o
     LEFT JOIN users u on o.user_id = u.user_id
![](https://i.imgur.com/2VvHf9q.png)

	users > orders
![](https://i.imgur.com/1ozNtTo.png)
‼ ⚠ 둘다 user_id 라는 fieldName 으로 LEFT JOIN 한 것이지만 데이터의 모습은 다르다.
	   원하는 조건을 고려하여 쿼리문을 작성해야한다.


#### INNER JOIN
- SELECT * FROM users u
	INNER JOIN orders o ON u.user_id = o.user_id
![](https://i.imgur.com/2sT98J5.png)

- SELECT * FROM orders o
  INNER JOIN users u ON o.user_id = u.user_id
![](https://i.imgur.com/duZu1M5.png)
‼ ⚠  INNER JOIN 은 공통된 data 를 추출하기 때문에 같은 field를 보여주지만 
		field의 순서가 다르다.
		

### 예제2.
checkins Table 과 users Table 연결
#### LEFT JOIN ( 두 TABLE 은 users_id라는 같은 FieldName 이 있음)
- SELECT * FROM checkins c
  LEFT JOIN users u ON c.user_id = u.user_id
![](https://i.imgur.com/nRO4DWd.png)

- SELECT * FROM users u
  LEFT JOIN checkins c ON u.user_id = c.user_id
![](https://i.imgur.com/8oqpNKy.png)

#### INNER JOIN
-  SELECT * FROM users u
   INNER JOIN checkins c ON u.user_id = c.user_id
![](https://i.imgur.com/JLpxRWj.png)


### 예제 3.
enrolleds Table 과 courses Table 연결
#### LETF JOIN ( 두 TABLE 은 course_id라는 같은 FieldName 이 있음)
- SELECT * FROM enrolleds e
  LEFT JOIN courses c ON e.course_id = c.course_id
![](https://i.imgur.com/4ezxkbf.png)

- SELECT * FROM courses c
  LEFT JOIN enrolleds e ON c.course_id = e.course_id
![](https://i.imgur.com/FXKb33i.png)

#### INNER JOIN
- SELECT * FROM enrolleds e
  INNER JOIN courses c ON c.course_id = e.course_id 
  ![](https://i.imgur.com/FUdOZyt.png)

