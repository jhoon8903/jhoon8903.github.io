---
layout: post
title: Flask mongodb data 삭제
subtitle: Python의 Flask를 이용한 API통신
categories: Python
author: Daniel
tags: 
 - Python
 - Network
 - Programming
banner: 
 image : https://play-lh.googleusercontent.com/keVVojxW-b11NTKWZg8GulfLlhqBpATvqGFViblYsI0fxW_8a0sIPgyRlB94Gu1AQMY
---

## Flask를 통해 등록된 게시글의 삭제

#### 조건

1.  card-box에 삭제버튼생성
2.  삭제버튼 클릭시 게시글이 사라져야함

#### 방안생각

1.  $('#card-box').hide() 로 카드 숨기기
2.  $('#card-box').empty() 로 카드 지우기
3.  app.route('/'), methods=["POST"] 로 DB에서 데이터 제거

#### 결과

1.  page reload시에 카드 다시 나타남
2.  1.  번과 동일 현상
    
    -   db상에 살아있는 data가 reload 되면서 나타나는 문제
3.  db에서 data가 삭제되어 reload시에도 현상유지

-   조건으로 보면 1,2 번 방안도 삭제 버튼 클릭시 카드가 없어지기 때문에 조건에는 부합하나 reload시에 다시 원상복귀 되어 확실한 방법인 3번으로 진행

### 결과도출과정

1.  삭제버튼 클릭시에 작동하는 여러가지 방법이 있겠지만, 현재실력으로 할 수 있는 방법은 한정되어 내가 할 수 있는 방법 생각
    
2.  onclick 버튼 생성시 해당 글만 삭제 할 수 있는 방법 모색  
    1) 각 db값에 num을 매겨 num에 해당하는 card delete  
    - server에 다시 값을 생성해야하므로 작업소요 증가  
    - num 입력 후 삭제시 기능은 구동하지만 삭제와 입력을 반복하면서 len(movie_list)에서 num이 중복되는 문제가 발생하여 동일한 num을 가지고 있는 db가 같이 삭제 됨  
    - num[1, 2, 3, 4][1] 삭제 후 len이 3이 되면서 db.insert시에 num[4]가 생성되어 [4]를 삭제하면 db 2개가 동시에 삭제되는 문제  
    - 추가적으로 data_recieve와 data_give시에 정수와 '문자열' 구분에 있어 int()를 삼입위치를 찾는데 많은 시간 소요됨  
    
3.  기존에 들어있던 속성[title,desc,star,comment]을 사용하여 추가 data입력 및 중복소요가 없어서 해당 방법으로 진행
    
4.  sever 설정
    

```python
@app.route('/movie', methods=['DELETE'])
def movie_delete():
    comment_receive = request.form['comment_give']
    db.movies.delete_one({'comment': comment_receive})
    return jsonify({'msg': '삭제 완료!'})
```

-   methods=['POST']도 가능하나 (/movie) 값이 같아 충돌문제 발생  
    - POST 사용시에는 /movie 를 변경해주어야 함 ex.) /movieDelete
-   속성값 중 'comment'속성 사용하여 구분

5.  client 설정(temp_html에 '삭제'버튼 생성)

-   onclick 이벤트 발생시에 작동하도록 버튼에 'comment'속성값 부여

```html
<button type="button" onclick="card_delete('${comment}')">삭제</button>
```

-   card_delete script 생성

```python
function card_delete(a) {
         let comment = a
           $.ajax({
               type: 'DELETE',
               url: '/movie',
               data: {comment_give: comment},
               success: function (response) {
                   alert(response['msg'])
                   window.location.reload();
               }
           });
       }
```

-   삭제시 자동으로 reload 되어 void 공간이 보이지 않도록 조치

### Fail note

1.  Flask 이해도 부족으로 route methods[POST,DELETE]  
    활용에 어려움을 겪음
2.  client 버튼 생성시 'url'값으로 진행하다 버튼에 <' a href '> 가 들어가면 버튼이 작동하지 않는 문제 발생
3.  function script에서 매개변수값을 넣지 않아 통신이 되지 않는 문제 발생
4.  오타 오타 오타 오타 오타 `[] {} () '' "" `` . ,`