---
layout: post
title: 에러 핸들링
subtitle: Try Chtch 에러 핸들링 하기
categories: Programming
author: Daniel
tags: 
 - NodeJS
 - Programming
banner:
 image : https://velog.velcdn.com/images/jeeho102/post/5e053405-ca9d-47dc-a015-7b433aaa1478/image.png
---

## 에러 핸들링 (Error handling)
	- 에러를 관리하는 방법
	- 에상가능 에러와 불가능 에러로 구분, 일반적으로 예상치 못한 에러 발생률이 높다.

### try / catch
	- 서버에러를 방지하고자 예외처리를 진행하는 방법
	- 일반적으로 try ... catch문 작성
<img src = 'https://user-images.githubusercontent.com/114923190/209467407-a534e251-164b-4985-bbf7-510620624e5a.png' width = '500'>

### throw
	- 고의로 error를 발생시키는 방법
	- throw를 호출하면 그 즉시 현재 실행되고 있는 함수는 실행을 멈추게 됨
<img src = 'https://user-images.githubusercontent.com/114923190/209467431-9fcf0734-5fdd-4b0e-b48e-1c347db4ba3f.png' width = '500'>

### finally
	- try resource 확보를 위해 일정시점에 해당 resource 확보를 위해 사용
	- finally는 에러의 발생여부와 상관없이 언제든지(무조건) 실행됨
<img src = 'https://user-images.githubusercontent.com/114923190/209467440-d1ac8584-e222-469d-a6ae-d310739443d3.png' width = '500'>
