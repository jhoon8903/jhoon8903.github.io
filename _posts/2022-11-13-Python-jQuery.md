---
layout: post
title: Python_jQuery_Ajax
subtitle : Python Jquery 적용하기
author: Daniel
categories: Python
tags:
 - Python
 - Programming
 - Language
banner:
 image: https://blog.kakaocdn.net/dn/bwLdHi/btqAIEhUJXu/ZJYz3SlI9zCZCf6wmKYlBK/img.png
---

JQuery는 외우는거 아니다.
--

![](https://i.imgur.com/kDhijQV.png)

'class' 대신 'id'로 지정

'temp_html'값 지정 > let temp_html = `지정값(백팁)`  
'$('#id').append(temp_html) = id값을 html 추가  
* 필요에 따라 값을 지정하여 넣고 빼고 가능

## JQuery & Ajax

#### GET 방식 : data를 조회(read) 요청 할 때

#### POST 방식 : data를 생성(Create), 변경(update), 삭제(delete) 요청 할 때

ajax call code

```python
$.ajax({
	type: "GET"
    url: "",
    data: {},
    success: function (response) {
// 자주 등장하는 구문 //
// let rows = response['값']['값2'] //
// for (let i = 0; i < 값2.length; i++) { //
	})
```

이미지 바꾸기

```python
$('id.val').attr('src',url);
```

텍스트 바꾸기

```python
$('id.val').text('text.val');
```

페이지 로딩 후 호출하기

```python
$(document).ready(function () { 

});
```