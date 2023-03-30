---
title: MySQL 통계 GROUP BY
category: MySQL
author: "이정훈"
tags: [MySQL, database]
img : ":mysql.png"
comments_disable: true
meta_description: "My SQL Query"
---

# 범주의 통계를 내주는 GROUP BY
- 동일한 범주를 같는  Data를 Group으로 묶어, 범주별 통계를 내는 것
순서 (한번에 하면 좋지만 왜 이렇게 되는지 알아야 한다.)
1. SELECT * FROM users
	   - users Table 에서 전체 목록 추출
	   ![](https://i.imgur.com/rHF258c.png)

2. SELECT * FROM users GROUP BY  name
	   - users Table 의 name Field 를  GROUP으로 묶는다.
	   - group 으로 묶으면서 중복된 성씨를 제외한 각 항목을 볼 수 있다.
	   => GROUP name 이후 목록이 498개에서 54개로 줄어들게된다.
	   
GROUP BY name 전
![](https://i.imgur.com/QJsv9bv.png)
			
GROUP BY name 후
![](https://i.imgur.com/q0zDmif.png)

3.  SELECT name FROM users GROUP BY  name
		- field에서 name만 추출한다.
![](https://i.imgur.com/YXFkyB0.png)

4. SELECT name COUNT ( * )FROM users GROUP BY  name
	   -  name의 수량을 COUNT 해준다.
![](https://i.imgur.com/Py3eSjd.png)

전체 성씨 중 '남' 씨의 숫자를 알고 싶을 때
![](https://i.imgur.com/EFXWcMr.png)


## GROUP BY 가지고 놀기
#### MIN (최소값)
- SELECT week, min(likes)FROM checkins group by week
- week의 likes 의 최소값![](https://i.imgur.com/5IwFwJs.png)
  
#### MAX (최대값)
  - SELECT week, max(likes)FROM checkins group by week
  - week의 likes의 최대값![](https://i.imgur.com/J3BlwmD.png)

#### AVG (평균값)
- SELECT week, avg(likes)FROM checkins group by week
- week의 likes의 평균값 ![](https://i.imgur.com/7E9MdcU.png)
- 평균의 자릿수 반올림 ROUND(avg(filed), 자릿수) 
![](https://i.imgur.com/UbyKmfP.png)

![](https://i.imgur.com/S87Xrna.png)

![](https://i.imgur.com/14rPi6I.png)

#### SUM (합계)
- SUM( fieldName )
![](https://i.imgur.com/HX2S5fE.png)
